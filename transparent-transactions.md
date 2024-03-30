## 7.1.4 Understanding transparent transactions

Zcash supports two types of addresses: transparent (**t-addresses**) and shielded (**z-addresses**). Transparent transactions are like Bitcoin transactions, where the details of the transaction, including sender, receiver, and amount, are publicly visible on the blockchain. 

In a typical blockchain system like Bitcoin or Zcash transparent mode, a transaction includes inputs and outputs. Inputs are the references to previously received tokens (in Bitcoin's terminology, BTC; in Zcash, **ZEC**) that are being spent, while outputs are the new transactions created to transfer value to another address. Essentially, inputs unlock tokens to be spent, and outputs lock these tokens by assigning them to new owners, setting the stage for future transactions​.

The following diagram outlines the difference between transparent and shielded transactions:

```mermaid
graph TD
    B{Choose Transaction Type}
    B -- Transparent --> C[Input: Unlock ZEC]
    C --> D[Output: Lock ZEC to new owner]
    D --> E[Transaction Visible on Blockchain]
    B -- Shielded --> F[Input: zk-SNARKs Proof]
    F --> G[Output: Encrypted Transaction Details]
    G --> H[Transaction Details Hidden on Blockchain]

    style B fill: #fff,stroke:#FFA500,stroke-width:2px;
    style C fill: #fff,stroke:#FFA500,stroke-width:2px;
    style D fill: #fff,stroke:#FFA500,stroke-width:2px;
    style E fill: #fff,stroke:#FFA500,stroke-width:2px;
    style F fill: #fff,stroke:#FFA500,stroke-width:2px;
    style G fill: #fff,stroke:#FFA500,stroke-width:2px;
    style H fill: #fff,stroke:#FFA500,stroke-width:2px;
```

Here we are going to be looking at the transparent side, here are the main concepts around a transparent transaction:

- Transparent Addresses: These are similar to Bitcoin addresses, easily recognized because they start with a "t". They allow funds to be sent in a manner where the transaction details (sender, receiver, amount) are visible on the blockchain.
- Transaction Components: A transparent transaction typically consists of **inputs** and **outputs**:
    - Inputs: Reference previous transactions' outputs, unlocking specific amounts of ZEC that were previously received.
    - Outputs: Specify new recipients for ZEC and lock these amounts for future spending.
- UTXOs (Unspent Transaction Outputs): Zcash, like Bitcoin, uses a UTXO model for tracking balances. Each transaction output becomes a UTXO until it is spent as an input in a new transaction.
- Transaction Fees: Just like in Bitcoin, transaction fees in Zcash are deducted from the inputs, incentivizing miners to include transactions in the next block.

### Example

In the following scenario, we look into a more realistic course of transactions as they occur on the blockchain. While wallet software typically manages the internals of these processes, understanding the underlying mechanics offers valuable insights.

The process begins with Alice deciding to send 5 ZEC to Bob. This decision initiates the transaction sequence.

Alice reviews her available UTXOs to find one that can cover the amount she wants to send to Bob. She selects a UTXO valued at 6 ZEC, which is more than the amount she wishes to send.

Create Transaction (Alice):

- Input: Alice unlocks the 6 ZEC from her selected UTXO, indicating she's using this amount for the transaction. This action signifies her intention to transfer part of this amount to Bob.
- Output 1: She designates 5 ZEC to Bob's address. This output specifies the amount Bob will receive.
- Output 2: The remaining balance from the UTXO, minus the transaction fee, is sent back to Alice as change. Given the transaction fee is 0.001 ZEC, Alice's change amounts to 0.999 ZEC.

A small fee (0.001 ZEC) is deducted to cover the cost of processing the transaction. This fee is implicitly included in the transaction by reducing the change Alice receives.

Bob receives the 5 ZEC UTXO from Alice. He then decides to send 3 ZEC to Carol.

Bob uses the 5 ZEC UTXO received from Alice to fund his transaction to Carol.

Create Transaction (Bob):

- Input: Bob unlocks the 5 ZEC UTXO.
- Output 1: He sends 3 ZEC to Carol's address.
- Output 2: Bob receives change from the transaction. After deducting another transaction fee (0.001 ZEC), he is left with 1.999 ZEC.

Similar to Alice's transaction, Bob's transaction incurs a fee of 0.001 ZEC, reducing his change.

Carol now has a new UTXO worth 3 ZEC, and Bob retains a UTXO worth 1.999 ZEC as his change from the transaction.

The process concludes with both transactions successfully executed, fees paid, and new UTXOs created for Bob and Carol.

```mermaid
graph TD
    B["Alice decides to send 5 ZEC to Bob"]
    B --> C{"Alice's UTXOs"}
    C --> D["Select UTXO: 6 ZEC"]
    D --> E{"Create Transaction"}
    E -->|Input| F["Alice's Address (6 ZEC UTXO unlocked)"]
    F -->|Output 1| G["Bob's Address (5 ZEC locked)"]
    F -->|Output 2| H["Alice's Change Address (0.999 ZEC locked)"]
    G --> I["Bob now has 5 ZEC UTXO"]
    H --> J["Transaction Fee: 0.001 ZEC"]
    I --> K["Bob decides to send 3 ZEC to Carol"]
    K --> L{"Bob's UTXOs"}
    L --> M["Select UTXO: 5 ZEC"]
    M --> N{"Create Transaction"}
    N -->|Input| O["Bob's Address (5 ZEC UTXO unlocked)"]
    O -->|Output 1| P["Carol's Address (3 ZEC locked)"]
    O -->|Output 2| Q["Bob's New UTXO (1.999 ZEC change)"]
    P --> R["Carol now has 3 ZEC UTXO"]
    Q --> S["Transaction Fee: 0.001 ZEC"]
    R --> T["Bob retains 1.999 ZEC UTXO"]

    style B fill: #fff,stroke:#FFA500,stroke-width:2px;
    style C fill: #fff,stroke:#FFA500,stroke-width:2px;
    style D fill: #fff,stroke:#FFA500,stroke-width:2px;
    style E fill: #fff,stroke:#FFA500,stroke-width:2px;
    style F fill: #fff,stroke:#FFA500,stroke-width:2px;
    style G fill: #fff,stroke:#FFA500,stroke-width:2px;
    style H fill: #fff,stroke:#FFA500,stroke-width:2px;
    style I fill: #fff,stroke:#FFA500,stroke-width:2px;
    style J fill: #fff,stroke:#FFA500,stroke-width:2px;
    style K fill: #fff,stroke:#FFA500,stroke-width:2px;
    style L fill: #fff,stroke:#FFA500,stroke-width:2px;
    style M fill: #fff,stroke:#FFA500,stroke-width:2px;
    style N fill: #fff,stroke:#FFA500,stroke-width:2px;
    style O fill: #fff,stroke:#FFA500,stroke-width:2px;
    style P fill: #fff,stroke:#FFA500,stroke-width:2px;
    style Q fill: #fff,stroke:#FFA500,stroke-width:2px;
    style R fill: #fff,stroke:#FFA500,stroke-width:2px;
    style S fill: #fff,stroke:#FFA500,stroke-width:2px;
    style T fill: #fff,stroke:#FFA500,stroke-width:2px;
```

> [!TIP]
> Understanding Wallet Interaction with the Zcash Blockchain
>
> Ever wondered how the wallet on your phone communicates with the Zcash blockchain? Here's a brief overview:
>
> - Wallet applications connect to the blockchain through nodes or servers that maintain a complete transaction history. These nodes listen on specific ports for Remote Procedure Call (RPC) commands, allowing the wallet software to send transactions to the blockchain and receive data in response.
> - Within the Zcash ecosystem, there are primarily two types of node software:
>
>   - The original node, a fork from Bitcoin, modified and written in C++.
>   - The Zebra node, developed from scratch in Rust.
>
> - Both types of nodes offer RPC endpoints for communication with the blockchain. While it's anticipated that the Zebra node (`zebrad`) might become the main node in the future, examples here will focus on interacting with the Zebra node whenever possible.
> - To interact with the blockchain, such as sending transparent transactions, the `sendrawtransaction` RPC call is commonly used.
> - The standard port for Zcash Mainnet RPC methods is 8232. An example session might use the curl command on Linux to request data from the blockchain:

**Examples*:

Retrieve some blockchain data from the RPC interface.


Get blockchain information:
```bash
$ curl --silent --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getblockchaininfo", "params": [] }' -H 'Content-type: application/json' http://127.0.0.1:8232/ | jq
{
  "result": {
    "chain": "main",
    "blocks": 411181,
    "bestblockhash": "00000000018d76bfa07b64e32486ee0336afccbb18932925c99d97b3f1de299f",
    "estimatedheight": 2464100,
    "upgrades": {
      "5ba81b19": {
        "name": "Overwinter",
        "activationheight": 347500,
        "status": "active"
      },
      "76b809bb": {
        "name": "Sapling",
        "activationheight": 419200,
        "status": "pending"
      },
      "2bb40e60": {
        "name": "Blossom",
        "activationheight": 653600,
        "status": "pending"
      },
      "f5b9230b": {
        "name": "Heartwood",
        "activationheight": 903000,
        "status": "pending"
      },
      "e9ff75a6": {
        "name": "Canopy",
        "activationheight": 1046400,
        "status": "pending"
      },
      "c2d6d0b4": {
        "name": "NU5",
        "activationheight": 1687104,
        "status": "pending"
      }
    },
    "consensus": {
      "chaintip": "5ba81b19",
      "nextblock": "5ba81b19"
    }
  },
  "id": "curltest"
}
$
```

Get information for the block height `123`:
```bash
$ curl --silent --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getblock", "params": ["123"] }' -H 'Content-type: application/json' http://127.0.0.1:8232/ | jq
{
  "result": {
    "hash": "000092a5acc3cf903f0c6b0124d0db60646c17624021f5667d242fd038131e24",
    "confirmations": 411438,
    "height": 123,
    "tx": [
      "7ef714e1545d484a0cabe56db69acff925431d4855d209e37259e1fdd1bb1137"
    ],
    "trees": {}
  },
  "id": "curltest"
}
$
```
