## Overview

This subgraph code is designed to track token balances by address interacting and monitoring the Ethereum blockchain for events related to ERC-20 token transfers. It captures and processes `Transfer` events emitted by ERC-20 smart contracts and maintains data regarding token balances and accounts involved in these transfers.

## How to Get Token Balances by Address

You can use this subgraph to query and retrieve token balances for specific accounts (addresses) within the supported blockchains.

**Query the Subgraph:**

Use The Graph's query mechanism to retrieve token balances for a particular address

Here's an example query in GraphQL format:

```graphql
query {
  account(id: "YOUR_ACCOUNT_ADDRESS") {
    id
    balances {
      token {
        id
        name
        symbol
      }
      balance
    }
  }
}
```

## Key Components

### 1. `handleTransfer` Function

The core functionality of this subgraph is encapsulated in the `handleTransfer` function. This function is called whenever a `Transfer` event is emitted by an ERC-20 token contract. It performs the following tasks:

- **Ignoring Zero-Value Transfers:** It ignores transfers with a value of zero, as these transfers do not affect token balances.

- **Checking for Ignored Tokens:** It checks if the token address (the address of the ERC-20 contract) is on the ignore list. If it is, the function returns without further processing.

- **Getting or Creating ERC-20 Token Entity:** It retrieves the ERC-20 token entity associated with the event's token address. If the token does not exist, it creates one.

- **Ignoring Non-ERC-20 Tokens:** If the token entity is not found (null), it assumes the token address is not a valid ERC-20 token and adds it to the ignore list.

- **Getting or Creating Accounts:** It retrieves or creates account entities for both the sender (`from`) and the receiver (`to`) of the token transfer.

- **Updating Balances:** It updates the token balances of both the sender and the receiver based on the transfer. The balance is adjusted according to the transfer value.

### 2. Ignored Tokens

The subgraph maintains a list of token addresses that should be ignored (i.e., not processed). When a token is added to this list, the subgraph will skip processing any future transfers involving that token.

## Data Entities

The subgraph code utilizes various data entities to maintain information about ERC-20 tokens, accounts, and balances:

- `ERC20Token`: Represents ERC-20 tokens, including details such as symbol, name, and token address.

- `Account`: Represents Ethereum addresses that have interacted with ERC-20 tokens.

- `Balance`: Represents the token balance of an account for a specific ERC-20 token.

- `IgnoredToken`: Represents token addresses that should be ignored in future processing.
