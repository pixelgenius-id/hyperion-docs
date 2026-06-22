[:fontawesome-solid-arrow-left-long: Hyperion Configuration](hyperion_configuration.md#add-new-chain)
# Chain Configuration Reference

This section is a quick guide of the config.json file. Here you are going to find a brief explanation of each parameter
with its default value and an example of real usage.

### 1. API Configuration

- `"chain_name": "EXAMPLE Chain"`
- `"server_addr": "127.0.0.1"`
- `"server_port": 7000`
- `"server_name": "127.0.0.1:7000"`
- `"provider_name": "Example Provider"`
- `"provider_url": "https://example.com"`
- `"chain_logo_url": ""`
- `"enable_caching": true` ⇒ Set API cache
- `"cache_life": 1` ⇒ Define the cache life
- `"limits"` ⇒ Set API response limits
  - `"get_actions": 1000`
  - `"get_voters": 100`
  - `"get_links": 1000`
  - `"get_deltas": 1000`
- `"access_log": false` ⇒ Enable log API access.
- `"hot_first_actions": false` ⇒ When enabled, an unbounded newest-first `get_actions` account poll (e.g. `?account=vexcore&sort=desc`, no `before`/`after`, no pagination) searches only the most recent action partition(s) first, widening to the full index set **only** if that window returns fewer than `limit` hits. Keeps high-frequency polling for the latest actions off the older/warm shards. Default off; safe to enable without reindexing. *(Note: while a poll is served from the hot window, the response `total` reflects that window only.)*
- `"hot_first_window": 2` ⇒ Number of newest action partitions searched in the hot window when `hot_first_actions` **or** `hot_first_transaction` is enabled. Default `2` (covers the live partition plus the previous one across a rollover).
- `"hot_first_transaction": false` ⇒ When enabled, a `get_transaction` lookup **without** a `block_hint` searches the most recent action partition(s) first and widens to the full index set **only** on a miss. Without it, a `trx_id` lookup has no block range to prune on and fans out across every action partition — including cold/old shards. Because all of a transaction's documents share one block (one partition), a hot-window hit is always complete, so the common case (recent transactions) never touches the older tiers. Reuses `hot_first_window`. Default off; safe to enable without reindexing. *(Tip: clients that already know the block can pass `?block_hint=<block_num>` to target the single partition directly.)*
- `"explorer"` ⇒ Explorer configuration (see [Explorer Setup](../explorer.md))
    - `"upstream": "http://127.0.0.1:4777"` ⇒ URL of the Explorer SSR server
    - `"theme": "default"` ⇒ Theme name (matches `<name>.theme.mjs` in explorer/themes/)
    - `"home_redirect": true` ⇒ Redirect API root to `/explorer/`

### 2. Settings

- `"preview": false` ⇒ Preview mode - prints worker map and exit
- `"chain": "vex"` ⇒ Chain name (must match the name used in ecosystem.config.js)
- `"eosio_alias": "eosio"`
- `"parser": "1.8"` ⇒ Version of the parser to be used
- `"auto_stop": 300` ⇒ Automatically stop Indexer after X seconds if no more blocks are being processed (0=disable)
- `"index_version": "v1"` ⇒ Set the index version
- `"debug": false` ⇒ Enable debug mode
- `"rate_monitoring": true` ⇒ Print data rates for each phase every 5s
- `"bp_logs": false` ⇒ Enable logs
- `"bp_monitoring": false` ⇒ Allow monitoring and logging of missed blocks and rounds
- `"ipc_debug_rate": 60000` ⇒ interval between IPC messaging rate debug messages, setting it to false or 0 will disable
  the logging
- `"allow_custom_abi": false` ⇒ allow using custom ABIs from the custom-abi/<CHAIN_NAME> folder, they must match the
  pattern <contract>-<start_block>-<end_block>.json. Those ABIs will overwrite on-chain data for the given range.
- `"auto_mode_switch": false` ⇒ Automatically restart the indexer in data mode after ABI scan completes. **Set to `true` for first-time indexing** — see [Indexer Troubleshooting](../help/indexer.md#indexer-stuck-after-abi-scan) if the indexer appears stuck after ABI scan.
- `"index_partition_size": 10000000` ⇒ Number of blocks per Elasticsearch index partition. **Do not change after indexing has started.** See [Index Lifecycle Management](../operations/index_management.md)
- `"max_retained_blocks": 0` ⇒ Maximum blocks to retain on Elasticsearch. Older data is automatically pruned. Set to `0` to disable. See [Index Lifecycle Management](../operations/index_management.md)
- `"es_replicas": 0` ⇒ Number of Elasticsearch index replicas
- `"tiered_index_allocation"` ⇒ Tiered storage allocation rules for Elasticsearch indices. See [Index Lifecycle Management](../operations/index_management.md)

### 3. Blacklists

Blacklist for actions and deltas

- "actions": []
- "deltas": []

### 4. Whitelists

Whitelist for actions and deltas

- actions": []
- "deltas": []

### 5. Scaling options

- `"batch_size": 10000` ⇒ Parallel reader batch size in blocks
- `"queue_limit": 50000` ⇒ Queue size limit on rabbitmq
- `"readers": 1` ⇒ Number of readers
- `"ds_queues": 1` ⇒ Number of deserializer queues
- `"ds_threads": 1` ⇒ Number of deserializer threads
- `"ds_pool_size": 1` ⇒ Deserializer pool size
- `"indexing_queues": 1` ⇒ Number of indexing queues
- `"ad_idx_queues": 1` ⇒ Multiplier for action indexing queues
- `"max_autoscale": 4` ⇒ Max number of readers to autoscale
- `"auto_scale_trigger": 20000` ⇒ Number of itens on queue to trigger autoscale
- `"routing_mode": "heatmap"` ⇒ We recommend using the `heatmap` routing algorithm since it uses less memory. If you see
  performance issues on large chains with very unbalanced action distribution (majority of the action on a single
  contract) you can change it to `round_robin` to improve throughput. Keep in mind that the `round_robin` method will
  use more RAM as it has larger `ds_pool_size` values

### 6. Indexer configuration

- `"start_on": 0` ⇒ Start indexing on block (0=disable)
- `"stop_on": 0` ⇒ Stop indexing on block  (0=disable)
- `"rewrite": false` ⇒ Force rewrite the target replay range
- `"purge_queues": true` ⇒ Clear rabbitmq queues before starting the indexer
- `"live_reader": false` ⇒ Enable live reader
- `"live_only_mode": false` ⇒ Only reads realtime data serially
- `"abi_scan_mode": true` ⇒ Enable abi scan mode (<b>Indicated on the first run</b>)
- `"fetch_block": true` ⇒ Request full blocks from the state history plugin
- `"fetch_traces": true` ⇒ Request traces from the state history plugin
- `"disable_reading": false` ⇒ Completely disable block reading, for lagged queue processing
- `"disable_indexing": false` ⇒ Disable indexing
- `"process_deltas": true` ⇒ Read table deltas
- `"max_inline": 20` ⇒ Max inline actions depth to index

### 7. Features

- `"index_deltas": true` ⇒ Index common table deltas (see delta on definitions/mappings)
- `"index_transfer_memo": true` ⇒ Index transfers memo
- `"index_all_deltas": true` ⇒ Index all table deltas

#### 7.1 Streaming

Enable live streaming

- `"enable": false`
- `"traces": false`
- `"deltas": false`

#### 7.2 Tables

Tables to fetch

- `"proposals": true`
- `"accounts": true`
- `"voters": true`
- `"userres": false`
- `"delband": false`

### 8. Prefetch

- `"read": 50` ⇒ Stage 1 prefetch size
- `"block": 100` ⇒ Stage 2 prefetch size
- `"index": 500` ⇒ Stage 3 prefetch size

## Example

!!! tip For multiple chains, you should have one config file for each chain.

For Vexanium, create `chains/vex.config.json`. The example below shows a minimal working configuration:

````json
{
  "api": {
    "chain_name": "vex",
    "server_addr": "127.0.0.1",
    "server_port": 7000,
    "server_name": "127.0.0.1:7000",
    "provider_name": "Example Provider",
    "provider_url": "https://example.com",
    "chain_logo_url": "",
    "enable_caching": true,
    "cache_life": 1,
    "limits": {
      "get_actions": 1000,
      "get_voters": 100,
      "get_links": 1000,
      "get_deltas": 1000
    },
    "access_log": false,
    "hot_first_actions": false,
    "hot_first_window": 2,
    "hot_first_transaction": false,
    "explorer": {
      "upstream": "http://127.0.0.1:4777",
      "theme": "default",
      "home_redirect": true
    }
  },
  "settings": {
    "preview": false,
    "chain": "vex",
    "eosio_alias": "vexcore",
    "parser": "3.2",
    "auto_stop": 300,
    "index_version": "v1",
    "debug": false,
    "rate_monitoring": true,
    "bp_logs": false,
    "bp_monitoring": false,
    "ipc_debug_rate": 60000,
    "allow_custom_abi": false,
    "index_partition_size": 10000000,
    "max_retained_blocks": 0,
    "es_replicas": 0
  },
  "blacklists": {
    "actions": [],
    "deltas": []
  },
  "whitelists": {
    "actions": [],
    "deltas": []
  },
  "scaling": {
    "batch_size": 10000,
    "queue_limit": 50000,
    "readers": 2,
    "ds_queues": 2,
    "ds_threads": 1,
    "ds_pool_size": 1,
    "indexing_queues": 1,
    "ad_idx_queues": 1,
    "max_autoscale": 4,
    "auto_scale_trigger": 20000
  },
  "indexer": {
    "start_on": 1,
    "stop_on": 0,
    "rewrite": false,
    "purge_queues": true,
    "live_reader": false,
    "live_only_mode": false,
    "abi_scan_mode": false,
    "fetch_block": true,
    "fetch_traces": true,
    "disable_reading": false,
    "disable_indexing": false,
    "process_deltas": true,
    "max_inline": 20
  },
  "features": {
    "streaming": {
      "enable": true,
      "traces": true,
      "deltas": false
    },
    "tables": {
      "proposals": true,
      "accounts": true,
      "voters": true,
      "userres": false,
      "delband": false
    },
    "index_deltas": true,
    "index_transfer_memo": true,
    "index_all_deltas": true
  },
  "prefetch": {
    "read": 50,
    "block": 100,
    "index": 500
  }
}
````

[:fontawesome-solid-arrow-left-long: Hyperion Configuration](hyperion_configuration.md#add-new-chain){ .md-button }

---

## Vexanium Example

The following is a working configuration for the **Vexanium** mainnet (`vex`).

Key differences from a standard Antelope chain:

- `eosio_alias` and `system_contract` are `"vexcore"` (not `"eosio"`)
- `parser` is `"3.2"` (Antelope Spring)
- `custom_core_token` is `"VEX"`
- `"vexcore::onblock"` is blacklisted to reduce index noise
- `live_only_mode: true` — indexing starts from a snapshot, not from block 1

```json
{
  "api": {
    "chain_name": "Vexanium",
    "server_addr": "0.0.0.0",
    "server_port": 7000,
    "server_name": "vexascan.com",
    "provider_name": "Your Provider Name",
    "provider_url": "https://yoursite.com",
    "chain_logo_url": "https://vexascan.com/assets/vex-logo.png",
    "enable_caching": false,
    "custom_core_token": "VEX",
    "chain_api": "http://127.0.0.1:8888",
    "push_api": "http://127.0.0.1:8888",
    "limits": {
      "get_actions": 1000,
      "get_voters": 100,
      "get_links": 1000,
      "get_blocks": 500,
      "get_trx_actions": 200
    },
    "disable_tx_cache": true,
    "disable_rate_limit": true
  },
  "settings": {
    "chain": "vex",
    "system_contract": "vexcore",
    "eosio_alias": "vexcore",
    "parser": "3.2",
    "preview": false,
    "auto_mode_switch": false,
    "index_version": "v1",
    "auto_stop": 0,
    "index_partition_size": 10000000,
    "es_replicas": 0
  },
  "indexer": {
    "start_on": 409543200,
    "stop_on": 0,
    "live_reader": true,
    "live_only_mode": true,
    "abi_scan_mode": false,
    "fetch_block": true,
    "fetch_traces": true,
    "fetch_deltas": true,
    "process_deltas": true
  },
  "scaling": {
    "batch_size": 2000,
    "readers": 1,
    "ds_queues": 1,
    "ds_threads": 1,
    "ds_pool_size": 1,
    "indexing_queues": 1,
    "max_autoscale": 4
  },
  "blacklists": {
    "actions": ["vexcore::onblock"],
    "deltas": []
  },
  "features": {
    "streaming": {
      "enable": true,
      "traces": true,
      "deltas": true
    },
    "tables": {
      "proposals": true,
      "accounts": true,
      "voters": true,
      "permissions": true,
      "user_resources": true
    },
    "index_deltas": true,
    "index_transfer_memo": true,
    "index_all_deltas": true
  }
}
```

**Connections** (`config/connections.json`):

```json
"chains": {
  "vex": {
    "name": "Vexanium",
    "chain_id": "f9f432b1851b5c179d2091a96f593aaed50ec7466b74f89301f957a83e56ce1f",
    "http": "http://127.0.0.1:8888",
    "ship": [{"label": "primary", "url": "ws://127.0.0.1:8080"}]
  }
}
```

**Ecosystem config** (`ecosystem.config.js`):

```js
{
  name: 'vex-indexer',
  script: './hyp-control',
  args: 'start indexer vex'
},
{
  name: 'vex-api',
  script: './hyp-control',
  args: 'start api vex'
}
```
