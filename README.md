# GitHub Action - Runner Switch

This GitHub Action determines the current usage of GitHub Actions minutes for an organization and provides the option to switch to an alternative runner when the included minutes have been consumed.

## Inputs

### `token`

**Required** The token to request billing information. The token must have organization admin read permissions.

### `threshold`

The threshold value in percent of the included minutes from which the alternative runner is to be used. This input is optional and has a default value of 95%.

### `runner`

The GitHub runner to be used. This input is optional and has a default value of `ubuntu-latest`.

### `alternative_runner`

The runner to be used after the included GitHub minutes have been consumed. This input is optional and has a default value of `ubuntu-latest`.

## Outputs

### `total_minutes_used`

The minutes used throughout the last billing cycle.

### `total_paid_minutes_used`

The minutes paid throughout the last billing cycle.

### `included_minutes`

The minutes included in the organization's contract.

### `included_usage_percentage`

The percentage of the included minutes consumed throughout the billing cycle.

### `runner`

The runner to be used based on the consumed minutes.

## Example Usage

```yaml
uses: your-org/action-gh-runner-usage@v1
with:
  token: ${{ secrets.TOKEN }}
  threshold: 90
  runner: ubuntu-latest
  alternative_runner: buildjet-4vcpu-ubuntu-2204
```

This action can be used to switch to an alternative runner if the included GitHub minutes have been consumed up to a specified threshold value. In the example above, if the included usage percentage is greater than or equal to 90%, the action switches to the buildjet-4vcpu-ubuntu-2204 runner. Otherwise, it uses the default runner, ubuntu-latest.

## License

This project is licensed under the MIT License.
