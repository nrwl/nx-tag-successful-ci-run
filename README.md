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

## Advanced Configuration Option (not recommended for most users)

<!-- start configuration-options -->

```yaml
- uses: nrwl/nx-tag-successful-ci-run@v1
  with:
    # ADVANCED: You can override the command we use to derive the final tag if you feel strongly about it.
    #
    # The command is a string which will be provided to bash's echo command in order to form the final tag name.
    #
    # By default to make the tag more valuable, we include some dynamic data about the run such as the Github
    # Run ID and datetime. You can do something similar, or completely different. We prefix the tag name with `nx_`
    # to make it clear why this tag exists and where it orginated from to differentiate it from other tags you may
    # have in your repo.
    #
    # NOTE: Our default value is set up to work seamlessly with the complementary `nrwl/nx-set-shas` action, so if
    # you do customize this, it will be up to you to ensure that your tag is compatible with the pattern in use with
    # that action too.
    #
    # Default: nx_successful_ci_run__$(echo ${{ github.run_id }})__$(date +"%Y-%m-%d-%H%M")-UTC
    command-string-to-echo-as-tag-name: ""

    # By default all tags will be preserved, set this to a pattern to match tags against which will be deleted before the new tag is applied.
    #
    # E.g. if you use the default value for ${{ inputs.command-string-to-echo-as-tag-name }}, you could set your pattern here to "nx_successful_ci_run__*"
    # if you wanted to clean up all previous tags which this action created.
    #
    # Default: ''
    remove-tags-matching-pattern: ""
```

<!-- end configuration-options -->
