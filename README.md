# @jedmao/semantic-release-npm-github-config

[**semantic-release**](https://github.com/semantic-release/semantic-release) shareable config to publish npm packages with [GitHub](https://github.com).

<!-- prettier-ignore-start -->
<!-- markdownlint-disable -->
[![GitHub Actions](https://github.com/jedmao/semantic-release-npm-github-config/workflows/Node%20CI/badge.svg?event=push)](https://github.com/jedmao/semantic-release-npm-github-config/actions)
[![npm version](https://img.shields.io/npm/v/@jedmao/semantic-release-npm-github-config/latest.svg)](https://www.npmjs.com/package/@jedmao/semantic-release-npm-github-config)
<!-- markdownlint-restore -->
<!-- prettier-ignore-end -->

<!-- markdownlint-disable commands-show-output -->

## Plugins

This [shareable configuration](https://github.com/jedmao/semantic-release-npm-github-config/blob/master/.releaserc.json) uses the following plugins:

- [`@semantic-release/commit-analyzer`](https://github.com/semantic-release/commit-analyzer)
- [`@semantic-release/release-notes-generator`](https://github.com/semantic-release/release-notes-generator)
- [`@semantic-release/npm`](https://github.com/semantic-release/npm)
- [`@semantic-release/github`](https://github.com/semantic-release/github)
- [`@semantic-release/git`](https://github.com/semantic-release/git)

## Summary

- Provides an informative [git](https://github.com/semantic-release/git) commit message for the release commit that does not trigger continuous integration and conforms to the [conventional commits specification](https://www.conventionalcommits.org/) (e.g., "chore(release): 1.2.3 [skip ci]\n\nnotes").
- Creates a tarball that gets uploaded with each [GitHub release](https://github.com/semantic-release/github).
- Publishes the same tarball to [npm](https://github.com/semantic-release/npm).
- Commits the version change in `package.json`.
- Creates or updates a [changelog](https://github.com/semantic-release/changelog) file.

## Install

```bash
$ npm install --save-dev semantic-release @jedmao/semantic-release-npm-github-config
```

## Usage

The shareable config can be configured in the [**semantic-release** configuration file](https://github.com/semantic-release/semantic-release/blob/master/docs/usage/configuration.md#configuration):

```json
{
  "extends": "@jedmao/semantic-release-npm-github-config",
  "branch": "master"
}
```

## Configuration

Ensure that your CI configuration has the following **_secret_** environment variables set:
- [`GH_TOKEN`](https://github.com/settings/tokens) with [`public_repo`](https://developer.github.com/apps/building-oauth-apps/understanding-scopes-for-oauth-apps/#available-scopes) access.
- [`NPM_TOKEN`](https://docs.npmjs.com/cli/token)
- [`NODE_AUTH_TOKEN`](https://docs.npmjs.com/cli/token)

See each [plugin](#plugins) documentation for required installation and configuration steps.

### GitHub workflows

If you're [configuring a GitHub workflow](https://help.github.com/en/articles/configuring-a-workflow) you might want to do a test build matrix first and then publish only if those tests succeed across all environments. The following will do just that, immediately after something is merged into `master`.

```yml
name: Node CI

on:
  push:
    branches:
      - master

jobs:
  test:
    name: Test on node ${{ matrix.node }} and ${{ matrix.os }}

    runs-on: ${{ matrix.os }}

    strategy:
      matrix:
        node: [8, 10, 12]
        os:
          - ubuntu-latest
          - windows-latest
          - macOS-latest

    steps:
      - name: Preserve line endings
        run: git config --global core.autocrlf false
      - name: Checkout
        uses: actions/checkout@v1
      - name: Setup Node
        uses: actions/setup-node@v1
        with:
          node-version: ${{ matrix.node }}
      - name: Install & test/cover
        run: npm ci && npm run cover
        env:
          CI: true

  release:
    name: npm publish / GitHub release
    needs: test
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v1
      - name: Setup Node
        uses: actions/setup-node@v1
        with:
          node-version: 12
          registry-url: https://registry.npmjs.org/
      - name: Install
        env:
          CI: true
        run: npm ci
      - name: Build
        if: success()
        run: npm run build
      - name: Semantic Release
        if: success()
        env:
          GH_TOKEN: ${{ secrets.GH_TOKEN }}
          NODE_AUTH_TOKEN: ${{ secrets.NPM_TOKEN }}
          NPM_TOKEN: ${{ secrets.NPM_TOKEN }}
        run: npx semantic-release
```
