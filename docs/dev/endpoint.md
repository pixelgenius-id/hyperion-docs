# Hyperion Endpoint List

## Vexanium Mainnet

| API | Swagger docs | Stream |
| ---- | ---------- | ----------- |
| https://vexascan.com:8443 | [docs](https://vexascan.com:8443/v2/docs){:target="_blank"} | `https://vexascan.com:8443` (Socket.IO path `/stream`) |
| https://v2.vexascan.com:2096 | [docs](https://v2.vexascan.com:2096/v2/docs){:target="_blank"} | `https://v2.vexascan.com:2096` (Socket.IO path `/stream`) |

Block explorer: [vexascan.com](https://vexascan.com){:target="_blank"}

!!! note
    `https://vexascan.com` without a port serves the VexaScan explorer website, not the API.
    Use one of the API URLs above for `/v2/...` and `/v1/...` requests.

Check that an endpoint is up with the health route, for example:

```
curl -s https://vexascan.com:8443/v2/health
```
