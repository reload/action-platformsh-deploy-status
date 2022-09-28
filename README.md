# workflow-platformsh-deploy-status

If anything goes wrong when **deploying** (*NOT building*) a PlatformSH project, the deployer does not fail.
This is an issue, as it can give you the idea that Drupal config has applied successfully (or a whole host of other issues)

This simple GitHub Action workflow checks if a deploy-status file exists on the PR environment.
This file is generated, by adding the following to your `.platform.app.yaml`:

```yml
  # We run deploy hook after your application has been deployed and started.
  # We use set -e to cause a total fail if something goes wrong.
  # Otherwise, a deploy-status file will instead be populated with 1.
  # You can use a Github action to check if there was any issues with deploy.
  # This is necessary, as PlatformSH just fails silently, such as when
  # the Drupal config is invalid.
  deploy: |
    STATUS_FILE="$PLATFORM_DOCUMENT_ROOT/sites/default/files/deploy-status"
    rm -f "$STATUS_FILE"
    echo "0" > "$STATUS_FILE"
    set -e
    [ DOING YOUR OWN DEPLOY CHANGES, SUCH AS DRUSH DEPLOY]
    echo "1" > "$STATUS_FILE"
```

## Using the workflow

Example setup: https://github.com/reload/storypal/blob/main/.github/workflows/platformsh-deploy-test.yml

You also must create a `.platform-deploy-status-url` file in your repo, like this:

https://github.com/reload/storypal/blob/main/.github/workflows/.platform-deploy-status-url

You can see the setup of the workflow in (.github/workflows/platformsh-deploy-test.yml)[.github/workflows/platformsh-deploy-test.yml]
