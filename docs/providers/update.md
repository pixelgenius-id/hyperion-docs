### Update and Build

!!! attention
    Check the latest release notes [here](https://github.com/pixelgenius-id/hyperion-history-api/releases/latest){:target="_blank"} for any special instructions before updating.

Checkout the main branch or the specific tag you wish to update to, fetch new changes with `git pull` and run `npm ci` (it will update all packages and automatically build afterwards)

```shell
git checkout main
git pull
npm ci
```

After the rebuild, you need to restart the indexer and api.
```shell
cd ~/hyperion
./run [chain name]-indexer
./run [chain name]-api
```
