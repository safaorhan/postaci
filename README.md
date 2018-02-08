# PostaCI

| | | 
|-|-|
| ![postaci](https://user-images.githubusercontent.com/4990386/35919965-89ccc314-0c27-11e8-83bd-d5e143e91793.png) | PostaCI is a simple continous integration tool to run [postman](https://www.getpostman.com) tests through http requests, schedule the test runs and and see the results as badges like these: <br /><br /> ![](https://img.shields.io/badge/all%20passing-34/34-green.svg) ![](https://img.shields.io/badge/some%20failing-40/42-orange.svg) ![](https://img.shields.io/badge/mind%20blowing-error-red.svg)

## Installation

```
$ npm install -g postaci
```

## Features

### Git Support
PostaCI will fetch your postman collections from the git repositories you provide. It can clone and pull the repositories to the directories you provide. PostaCI uses [nodegit](https://github.com/nodegit/nodegit) to make git operations.

### Multiple Collections & Boxes
PostaCI, with the help of [newman](https://github.com/postmanlabs/newman), can run multiple postman collections. PostaCI lets you configure environment, global and data variables. Keep reading for further information.

### Badges
PostaCI generate badges with the help of [badges](https://github.com/badges/shields) library. You can use this badges in your repository.

### Scheduling
You can schedule your test runs in a cron-like manner. It uses [node-schedule](https://github.com/node-schedule/node-schedule) to trigger test runs.

### Webhooks
PostaCI provides a simple web server and lets you trigger runs, get results of runs and pull the git repository. So you can use github webhooks, post-release scripts, etc to create a simple continous integration flow.

We, in [Bilgetech](http://www.bilgetech.com.tr) use this feature to refresh the test collections after tests are updated and re-run the tests after a new version of the tested project is published.

## Usage

This library is intended to be used as a cli. Currently we don't support programmatic usage as a module.

```
$ postaci -c config.json
```

To get started with PostaCI, you need to do the following:

1. Prepare one config file defining your box entries
2. Prepare your folder structure for each box and put .postacirc file into it.

## Concepts

### Box
A box is a representation of your test collections which are living in the same repository. Each box can have multiple runnables and must have a .postacirc file in which you define all of your runnables.

### Runnable
A runnable is the sum of one collection file and the necessary files such as environment variables, global variables, data variables etc. Runnables can share files but may have side effects, so be careful about that.

![box-folder-structure](https://user-images.githubusercontent.com/4990386/35972761-27fdd732-0ce4-11e8-944a-cb5342cca28c.png)

### .postacirc

This file must be created and placed into the root of the each box. It should not be .gitignored.

The .postacirc file is a json object and the structure of it is as follows:

| Field | Type | Description | Required |
| ----- | ---- | ----------- | -------- |
| runnables | array | Configuration of each runnable | Required
| runnables[].name | string | A name to identify runnable. It should be **unique** among the runnables of a box. | Required |
| runnables[].collection | string | Path to the collection file relative to the box root. | Required |
| runnables[].environment | string | Path to the environment variables file relative to the box root. | Optional |
| runnables[].globals | string | Path to the global variables file relative to the box root. | Optional |
| runnables[].iterationData | string | Path to the data variables file relative to the box root. | Optional |
| runnables[].iterationCount | number | Number of iterations. | Optional |

### config.json

config.json is the file you put all of the necessary information. You should not add this file to repository since it has credential information and environment specific settings.

You can use the [config-sample.json](/config-sample.json) and modify it to suit your needs.

| Field | Type | Description | Required |
| ----- | ---- | ----------- | -------- |
| boxes | array | Configuration of each box | Required
| boxes[].address | string | A name to identify box. It should be **unique** among the boxes in the configuration file. | Required |
| boxes[].gitConfig | object | Settings of the git repo of the box. | Required |
| boxes[].gitConfig.remoteUrl | string | Remote url of your git repo. | Required |
| boxes[].gitConfig.localDir | string | The directory of your box. **It will be used to clone your box folder.** | Required |
| boxes[].gitConfig.branch | string | The branch that will used to checkout and run the tests. | Required |
| boxes[].gitConfig.credentials | object | Your git credentials that will be used to clone and pull. | Required |
| boxes[].gitConfig.credentials.username | string | Your git username. | Required |
| boxes[].gitConfig.credentials.passphrase | string | Your git password. | Required |
| boxes[].injectAssets | number | The information to be used to inject assets variable. | Optional |
| boxes[].injectAssets.key | number | The key of assets variable | Required if injectAssets exists. |
| boxes[].injectAssets.value | number | The relative filepath of the root of your assets. | Required if injectAssets exists. |
| boxes[].scheduleConfig | string | Cron-like configuration. See [node-schedule](https://github.com/node-schedule/node-schedule#cron-style-scheduling) | Optional |

## What does postacı mean?

The word postacı `/postadʒɯ/` is the Turkish word for a postman. If you have hard time reading phonetic alphabet, you can call it post-uh-gee or post-uh-c-i.
