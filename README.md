# GitHub Action - Runner Switch

This GitHub Action determines the current usage of GitHub Actions minutes for an organization and provides the option to switch to an alternative runner when the included minutes have been consumed.

## Inputs

### `token`

**Required** The token to request billing information. The token must have organization admin read permissions.

### `runner`

The GitHub runner to be used. This input is optional and has a default value of `ubuntu-latest`.

### `alternative_runner`

The runner to be used after the included GitHub minutes have been consumed. This input is optional and has a default value of `ubuntu-latest`.

## Outputs

### `runner`

The runner to be used based on the consumed minutes.

## Example Usage

Create a workflow in your project that updates the runner to be used:

```yaml
name: Determine runner

on:
  workflow_dispatch:
  schedule:
    - cron: "0 8-18 * * 1-5" # this checks which runner to be used hourly on workdays between 8 and 18h (UTC). Adapt to your hours!!

jobs:
  determine_runner:
    runs-on: ubuntu-latest
    steps:
      - id: small_runner
        uses: boredland/action-gh-runner-usage@main
        with:
          token: ${{ secrets.GH_TOKEN_BILLING }}
          alternative_runner: buildjet-2vcpu-ubuntu-2204
      - id: big_runner
        uses: boredland/action-gh-runner-usage@main
        with:
          token: ${{ secrets.GH_TOKEN_BILLING }}
          alternative_runner: buildjet-4vcpu-ubuntu-2204
      - name: set runner variables
        run: |
          echo ${{ secrets.GH_TOKEN_BILLING }} | gh auth login --with-token
          echo ${{ steps.small_runner.outputs.runner }} | gh variable set small_runner --repo ${{ github.repository }}
          echo ${{ steps.big_runner.outputs.runner }} | gh variable set big_runner --repo ${{ github.repository }}
```

In your workflows use the runner variable:

```
runs-on: ${{ vars.small_runner }}

or

runs-on: ${{ vars.big_runner }}
```

This action can be used to switch to an alternative runner if the included GitHub minutes have been consumed up to a specified threshold value.

## License

This project is licensed under the MIT License.
