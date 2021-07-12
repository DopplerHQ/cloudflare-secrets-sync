# Cloudflare Secrets Sync Scripts

Scripts for syncing Doppler secrets to Cloudflare Workers and Cloudflare Pages.

While we continue to work towards a native Doppler integrated solution, we are providing bash scripts that customers can use in CI/CD systems to automate the syncing of Doppler secrets to Cloudflare Worker secrets or Cloudflare Page environment variables using Cloudflare's v4 API.

## Sync Doppler Secrets to Cloudflare Pages Environment Variables

[Bash script](./bin/cloudflare-pages-secrets-sync.sh) to sync secrets from Doppler to Cloudflare Pages using the [Pages Update Project API](https://api.cloudflare.com/#pages-project-update-project).

### Usage

The following enviroment variables must be set or will be prompted for when running the script.

> NOTE: Unfortunately, the Global API key must be used (instead of API tokens) as Cloudflare's API does not yet have API token permissions assignable for Cloudflare Pages Project updates.

- `CLOUDFLARE_ACCOUNT_ID`: Account Id
- `CLOUDFLARE_EMAIL`: Account email
- `CLOUDFLARE_GLOBAL_API_KEY`: Must be the global API key as APi token permissions don't yet exist

Cloudflare Pages Config

- `CLOUDFLARE_PAGES_NAME`: Name of your Cloudflare Pages application
- `CLOUDFLARE_PAGES_ENVIRONMENT` (preview | production): Which envirionment to sync secrets to

You can also optionally set the following environment variables for additional behavior:

- `IMPORT_CLOUDFLARE=yes`: Will import the current environment variables from Cloudflare to Doppler before syncing (only use this once)
- `CLEAN_SYNC=yes`: Doppler is the source of truth and any environment variables not in Doppler will be deleted
- `AUTO_DEPLOY=yes`: Trigger a deploy in order to apply the environment variable updates.

Once the required environment variables are set, secrets can be synced in a single command:

```sh
CLOUDFLARE_PAGES_NAME=your-pages-app \
CLOUDFLARE_PAGES_ENVIRONMENT=production \
AUTO_DEPLOY=yes \
./bin/cloudflare-pages-secrets-sync.sh
```

To also trigger a production deployment to immediately apply the secret updates, just add `AUTO_DEPLOY=yes`:

```sh
CLOUDFLARE_PAGES_NAME=your-pages-app \
CLOUDFLARE_PAGES_ENVIRONMENT=production \
AUTO_DEPLOY=yes \
./bin/cloudflare-pages-secrets-sync.sh
```
