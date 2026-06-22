# How to use

## 1. JavaScript client using https

Fetching data from Hyperion using JavaScript is straightforward with the built-in `https` module.

```javascript
const https = require('https');
```

In this example we fetch a Vexanium transaction by ID:

```
"https://vexascan.com/v2/history/get_transaction?id=f9b484a87f34e919c82128d61a75721b33b5643682c5b2a0cc57137961805fb6"
```

Create a Promise function that accepts the tx id and returns the result:

```javascript
let getTransaction = function (args) {
    let url = "https://vexascan.com/v2/history/get_transaction?id=" + args;
    return new Promise(function (resolve) {
        https.get(url, (resp) => {
            let data = '';
            resp.on('data', (chunk) => { data += chunk; });
            resp.on('end', () => { resolve(JSON.parse(data)); });
        });
    });
};
```

Call the function:

```javascript
(async () => {
    let id = "f9b484a87f34e919c82128d61a75721b33b5643682c5b2a0cc57137961805fb6";
    getTransaction(id).then(data => { console.log(data); });
})();
```

Example response:

```json
{
  "query_time_ms": 36.0,
  "executed": true,
  "trx_id": "f9b484a87f34e919c82128d61a75721b33b5643682c5b2a0cc57137961805fb6",
  "lib": 409662122,
  "actions": [
    {
      "action_ordinal": 1,
      "act": {
        "account": "arsitektoken",
        "name": "transfer",
        "data": {
          "from": "45g2lkptw412",
          "to": "swap.wind",
          "quantity": "1.379918 ARSI",
          "memo": "swap:0:14"
        }
      },
      "@timestamp": "2026-06-22T10:30:29.000",
      "block_num": 409662445
    }
  ]
}
```

<br>

## 2. Third party library

There is a third party JavaScript library for Hyperion: [https://github.com/eoscafe/hyperion-api](https://github.com/eoscafe/hyperion-api){:target="_blank"}

!!! note
    This is a third party library and is not maintained by the Vexanium team.
