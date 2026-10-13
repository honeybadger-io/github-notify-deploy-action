# Honeybadger Notify Deploy Action

Sending deployment notifications to Honeybadger allows your team to associate spikes in errors to changes that were deployed.

## Requirements

The action runs `curl` with `sh` on the runner. GitHub-hosted runners have both. On a self-hosted runner, or in a job that uses a `container`, make sure both are installed.

## Failure behavior

In `v2`, the action fails the step when the Honeybadger API returns an HTTP error (4xx or 5xx). For example, an invalid API key now makes the job fail.

In `v1`, the action only fails when the request cannot be sent (for example, a network error). HTTP errors from the API do not fail the step.

## Upgrading from v1

Change `@v1` to `@v2` in your workflow. The inputs are the same.

The `v1` tag keeps the old behavior. You only get the new behavior when you change to `@v2`.

If you do not want a failed deploy notification to fail your job, add `continue-on-error: true` to the step.

## Inputs

Use these inputs to customise the action.

Input Name | Default | Required? | Description
------------ | ------------- | ------------ | -------------
api_key | N/A | Y | The Honeybadger project API key
environment | production | N | The deployment environment
endpoint | https://api.honeybadger.io/v1/deploys | N | The deploy submission endpoint

## Example

```yaml
on:
  push:
    branches:
      - master

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v1
    - uses: honeybadger-io/github-notify-deploy-action@v2
      with:
        api_key: ${{ secrets.HONEYBADGER_API_KEY }}
```
