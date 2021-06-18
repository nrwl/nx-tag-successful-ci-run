# nx-tag-successful-ci-run

## Example Usage

**.github/workflows/ci.yml**

<!-- start example-usage -->
```yaml
# ... more CI config ...

jobs:
  primary:
    runs-on: ubuntu-latest
    name: Primary
    steps:
      - uses: actions/checkout@v2
        with:
          # We need to fetch all branches and commits so that Nx affected has a base to compare against.
          fetch-depth: 0

      # ... more steps here ...

      # If all the preceding steps were successful the following step and action will run and tag the repo
      # using the default pattern (recommended):

      - name: Tag main branch if all jobs succeed
        if: ${{ github.event_name == 'push' && github.ref == 'refs/heads/main' }} # If you use name other than "main" for your main branch, such as master, be sure to update this
        uses: nrwl/nx-tag-successful-ci-run@v1

      # ... more CI config ...

```
<!-- end example-usage -->

## Configuration Options

<!-- start configuration-options -->
```yaml
- uses: nrwl/nx-tag-successful-ci-run@v1
  with:
    # A string which will be provided to bash's echo command in order to form the final tag name.
    #
    # By default to make the tag more valuable, we include some dynamic data about the run such as the Github Run ID and datetime.
    # 
    # NOTE: If you customize this, be sure that your new pattern is compatible with the complementary `nrwl/nx-set-shas` action.
    #
    # Default: nx_successful_ci_run__$(echo ${{ github.run_id }})__$(date +"%Y-%m-%d-%H%M")-UTC
    command-string-to-echo-as-tag-name: ''
```
<!-- end configuration-options -->