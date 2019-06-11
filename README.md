# @jedmao/semantic-release-npm-github-config

[**semantic-release**](https://github.com/semantic-release/semantic-release) shareable config to publish npm packages with [GitHub](https://github.com).

[![Travis](https://img.shields.io/travis/jedmao/semantic-release-npm-github-config.svg?style=flat-square)](https://travis-ci.org/jedmao/semantic-release-npm-github-config)
[![npm version](https://img.shields.io/npm/v/@jedmao/semantic-release-npm-github-config/latest.svg?style=flat-square)](https://www.npmjs.com/package/@jedmao/semantic-release-npm-github-config)

## Plugins

This shareable configuration use the following plugins:

- [`@semantic-release/commit-analyzer`](https://github.com/semantic-release/commit-analyzer)
- [`@semantic-release/release-notes-generator`](https://github.com/semantic-release/release-notes-generator)
- [`@semantic-release/npm`](https://github.com/semantic-release/npm)
- [`@semantic-release/github`](https://github.com/semantic-release/github)
- [`@semantic-release/git`](https://github.com/semantic-release/git)

## Install

```bash
$ npm install --save-dev semantic-release semantic-release-npm-github-config
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

See each [plugin](#plugins) documentation for required installation and configuration steps.
