---
description: This tutorial will demonstrate how to build a subgraph and deploy it locally
---

# Building & Deploying Subgraph (local node)

## Install the graph-cli

[https://github.com/graphprotocol/graph-cli#installation](https://github.com/graphprotocol/graph-cli#installation)

## Build your own graph-indexer local node

Copy the below into a `docker-compose.yml` file

{% tabs %}
{% tab title="mainnet" %}
```yaml
version: "3"
services:
  graph-node:
    container_name: fra_indexer
    image: graphprotocol/graph-node:latest
    ports:
      - "8000:8000"
      - "8001:8001"
      - "8020:8020"
      - "8030:8030"
      - "8040:8040"
    depends_on:
      - ipfs
      - postgres
    environment:
      postgres_host: postgres
      postgres_user: graph-node
      postgres_pass: let-me-in
      postgres_db: graph-node
      ipfs: "ipfs:5001"
      GRAPH_ETH_CALL_BY_NUMBER: 1
      GRAPH_NO_EIP_1898_SUPPORT: 1
      GRAPH_ALLOW_NON_DETERMINISTIC_IPFS: 1
      GRAPH_ETHEREUM_GENESIS_BLOCK_NUMBER: 1425000
      ethereum: 'mainnet:no_eip1898,archive,traces:https://rpc-mainnet.findora.org'
      RUST_LOG: info
  ipfs:
    container_name: ipfs
    image: ipfs/go-ipfs:v0.4.23
    ports:
      - "5001:5001"
    volumes:
      - ./data/ipfs:/data/ipfs
  postgres:
    image: postgres
    ports:
      - "5432:5432"
    command: ["postgres", "-cshared_preload_libraries=pg_stat_statements"]
    environment:
      POSTGRES_USER: graph-node
      POSTGRES_PASSWORD: let-me-in
      POSTGRES_DB: graph-node
    volumes:
      - ./data/postgres:/var/lib/postgresql/data
```
{% endtab %}

{% tab title="testnet" %}
```
version: "3"
services:
  graph-node:
    container_name: fra_indexer
    image: graphprotocol/graph-node:latest
    ports:
      - "8000:8000"
      - "8001:8001"
      - "8020:8020"
      - "8030:8030"
      - "8040:8040"
    depends_on:
      - ipfs
      - postgres
    environment:
      postgres_host: postgres
      postgres_user: graph-node
      postgres_pass: let-me-in
      postgres_db: graph-node
      ipfs: "ipfs:5001"
      GRAPH_ETH_CALL_BY_NUMBER: 1
      GRAPH_NO_EIP_1898_SUPPORT: 1
      GRAPH_ALLOW_NON_DETERMINISTIC_IPFS: 1
      GRAPH_ETHEREUM_GENESIS_BLOCK_NUMBER: 1425000
      ethereum: 'mainnet:no_eip1898,archive,traces:https://prod-testnet.prod.findora.org:8545'
      RUST_LOG: info
  ipfs:
    container_name: ipfs
    image: ipfs/go-ipfs:v0.4.23
    ports:
      - "5001:5001"
    volumes:
      - ./data/ipfs:/data/ipfs
  postgres:
    image: postgres
    ports:
      - "5432:5432"
    command: ["postgres", "-cshared_preload_libraries=pg_stat_statements"]
    environment:
      POSTGRES_USER: graph-node
      POSTGRES_PASSWORD: let-me-in
      POSTGRES_DB: graph-node
    volumes:
      - ./data/postgres:/var/lib/postgresql/data
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Note: Findora's genesis begins at block number `1,425,000` as specified above via `GRAPH_ETHEREUM_GENESIS_BLOCK_NUMBER` env variable.
{% endhint %}

Run your indexer node

```bash
docker-compose up -d
Creating network "thegraph_default" with the default driver
Creating ipfs ...
Creating postgres ...
Creating postgres done
Creating ipfs ... done
Creating indexer ... 
Creating indexer ... done
```

`docker logs fra_indexer -f` should show the indexer synching with the Findora EVM chain

```bash
docker logs fra_indexer -f
Jul 29 05:33:22.849 INFO Graph Node version: 0.23.1 (2021-06-23)
Jul 29 05:33:22.861 INFO Generating configuration from command line arguments
Jul 29 05:33:22.909 INFO Starting up
Jul 29 05:33:22.911 INFO Trying IPFS node at: http://ipfs:5001/
Jul 29 05:33:22.928 INFO Creating transport, capabilities: archive, traces, url: https://api.s0.pops.one, provider: mainnet-rpc-0
Jul 29 05:33:22.948 INFO Successfully connected to IPFS node at: http://ipfs:5001/
Jul 29 05:33:23.680 INFO Connecting to Postgres, weight: 1, conn_pool_size: 10, url: postgresql://graph-node:HIDDEN_PASSWORD@postgres:5432/graph-node, pool: main, shard: primary
Jul 29 05:33:23.716 INFO Pool successfully connected to Postgres, pool: main, shard: primary, component: Store
Jul 29 05:33:23.721 INFO Setting up fdw, pool: main, shard: primary, component: ConnectionPool
Jul 29 05:33:23.728 INFO Running migrations, pool: main, shard: primary, component: ConnectionPool
Jul 29 05:33:24.748 INFO Migrations finished, pool: main, shard: primary, component: ConnectionPool
Jul 29 05:33:24.748 INFO Mapping primary, pool: main, shard: primary, component: ConnectionPool
Jul 29 05:33:24.767 INFO Connecting to Ethereum to get network identifier, capabilities: archive, traces, provider: mainnet-rpc-0
Jul 29 05:33:25.030 INFO Connected to Ethereum, capabilities: archive, traces, network_version: 0x6357d2e0, provider: mainnet-rpc-0
Jul 29 05:33:25.117 INFO Creating LoadManager in disabled mode, component: LoadManager
Jul 29 05:33:25.119 INFO Starting block ingestors
Jul 29 05:33:25.119 INFO Starting block ingestor for network, network_name: mainnet
Jul 29 05:33:25.120 INFO Starting job runner with 1 jobs, component: JobRunner
Jul 29 05:33:25.126 INFO Starting JSON-RPC admin server at: http://localhost:8020, component: JsonRpcServer
Jul 29 05:33:25.130 INFO Starting GraphQL HTTP server at: http://localhost:8000, component: GraphQLServer
Jul 29 05:33:25.130 INFO Starting index node server at: http://localhost:8030, component: IndexNodeServer
Jul 29 05:33:25.130 INFO Started all subgraphs, component: SubgraphRegistrar
Jul 29 05:33:25.130 INFO Starting metrics server at: http://localhost:8040, component: MetricsServer
Jul 29 05:33:25.131 INFO Starting GraphQL WebSocket server at: ws://localhost:8001, component: SubscriptionServer
Jul 29 05:33:25.258 INFO Downloading latest blocks from Ethereum. This may take a few minutes..., provider: mainnet-rpc-0, component: BlockIngestor
Jul 29 05:33:33.600 INFO Syncing 4 blocks from Ethereum., code: BlockIngestionStatus, blocks_needed: 4, blocks_behind: 4, latest_block_head: 12886125, current_block_head: 12886121, provider: mainnet-rpc-0, component: BlockIngestor
Jul 29 05:33:35.252 INFO Syncing 1 blocks from Ethereum., code: BlockIngestionStatus, blocks_needed: 1, blocks_behind: 1, latest_block_head: 12886126, current_block_head: 12886125, provider: mainnet-rpc-0, component: BlockIngestor
```

A few components are installed

Management: [http://localhost:8020/](http://localhost:8020/) where subgraph are being created/deployed/deleted

Metrics / playground: [http://localhost:8030/](http://localhost:8030/)

Visit your playground using the URL [http://127.0.0.1:8030/graphql/playground](http://127.0.0.1:8030/graphql/playground) and start playing with graphQL API.

An example of query you can use to show subgraph currently being indexed would be :

```graphql
query {
  indexingStatuses {
    subgraph
    entityCount
    fatalError {
      handler
    }
    nonFatalErrors {
      handler
    }
    chains {
      network
      latestBlock {
        number
      }
      chainHeadBlock {
        number
      }
      lastHealthyBlock {
        number
      }
    }
  }
}
```

Right now, of course, the result is empty

```graphql
{
  "data": {
    "indexingStatuses": []
  }
}
```

## Our first subgraph

Lets use blocklytics/ethereum-blocks subgraph as example

```bash
git clone https://github.com/FindoraNetwork/ethereum-blocks
cd ethereum-blocks
```

Inside `package.json` you'll find the following script commands. There's no need to update these unless your localhost is different.

```json
"create-findora": "graph create --node http://localhost:8020 findora/blocks",
"deploy-findora": "graph deploy --node http://localhost:8020 --ipfs http://localhost:5001 findora/blocks",
```

### Update the manifest

Update the manifest `subgraph.yaml` file `datasources[0]['network']` to `mainnet` or `testnet` accordingly to how you edited the network in your `docker-compose.yaml`\


If there is no need to index the entire blockchain, you can update the attribute `startBlock` to speed up the sync : `datasources[0]['source']['startBlock']`

{% hint style="info" %}
It is highly recommended to minimize the number of blocks to be indexed to avoid putting load on the RPCs and to speed up the usage of your subgraph/application
{% endhint %}

### Create and deploy the subgraph

```bash
yarn
yarn codegen
yarn build
yarn create-findora 
yarn deploy-findora
```

Sync begins and you are good to query your subgraph [http://localhost:8000/subgraphs/name/findora/blocks/graphql](http://localhost:8000/subgraphs/name/findora/blocks/graphql)

Note that the above example query should now show your something similar to the below

```graphql
{
  "data": {
    "indexingStatuses": [
      {
        "chains": [
          {
            "chainHeadBlock": {
              "number": "12887470"
            },
            "lastHealthyBlock": null,
            "latestBlock": {
              "number": "10701810"
            },
            "network": "testnet"
          }
        ],
        "entityCount": "1812",
        "fatalError": null,
        "nonFatalErrors": [],
        "subgraph": "QmVDqqTJZM3ZHizSubG2H5b1vQ1Jp7iMD6Sc94A9zyRnAK"
      }
    ]
  }
}
```

`data['indexingStatuses'][0]['chains'][0]['latestBlock']['number']` will indicate the last block synched

{% hint style="warning" %}
check your indexer log if the sync is stuck `docker logs indexer -f --since 1m`
{% endhint %}

## How to write subgraphs

### Official Graph's doc

[https://thegraph.com/docs/developer/create-subgraph-hosted](https://thegraph.com/docs/developer/create-subgraph-hosted)

### Community article

[https://medium.com/protofire-blog/subgraph-development-part-1-understanding-and-aggregating-data-ef0c9a61063d](https://medium.com/protofire-blog/subgraph-development-part-1-understanding-and-aggregating-data-ef0c9a61063d)

[https://medium.com/protofire-blog/subgraph-development-part-2-handling-arrays-and-identifying-entities-30d63d4b1dc6](https://medium.com/protofire-blog/subgraph-development-part-2-handling-arrays-and-identifying-entities-30d63d4b1dc6)

### Getting help

the Graph’s Discord server [https://discord.gg/vtvv7FP](https://discord.gg/vtvv7FP)
