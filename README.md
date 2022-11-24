# action-platformsh-deploy-status

If anything goes wrong when **deploying** (*NOT building*) a PlatformSH project, the deployer does not fail.
This is an issue, as it can give you the idea that Drupal config has applied successfully (or a whole host of other issues)

## The deploy-status file

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
    [ .. DOING YOUR OWN DEPLOY CHANGES, SUCH AS DRUSH DEPLOY .. ]
    echo "1" > "$STATUS_FILE"
```

In this example, we're putting the file in `/sites/default/files/` as it's a Drupal site.

The files folder is the only folder that is writable during build.

## Getting the ready URL

This action relies on another action, for getting the environment URL.

https://github.com/reload/action-platformsh-url

Most of the inputs here, are needed for that action.
