# CompanyDAO.org API Documentation

The base URL to connect to the API is `https://api.companydao.org`.

## Authentication
All API requests require authentication. You need to pass your API key in the Authorization header as a Bearer token. 

```
Authorization: Bearer <Your API Key>
```

## Networks
CompanyDAO.org operates on two blockchain networks - Ethereum (ETH) and Polygon (Matic).

## Blockchain Transactions
Certain POST requests will produce blockchain transactions, which are relayed through our system. The relayer's balance must have enough funds to send transactions. To check the relayer's information, use the `getRelayerInfo` endpoint.

```
GET /relayer
```
Response:
```json
[
  {
    "address": "<Relayer Address on Chain 1>",
    "balance": "<Relayer Balance on Chain 1>",
    "chainId": "<Chain ID 1>"
  },
  {
    "address": "<Relayer Address on Chain 2>",
    "balance": "<Relayer Balance on Chain 2>",
    "chainId": "<Chain ID 2>"
  }
]
```

For each POST request that produces a transaction, the response will include the transaction hash (`tx_hash`), the chain ID (`chainId`), and a URL to view the transaction on Etherscan (for Ethereum transactions) or Polygonscan (for Polygon transactions).

Response example:
```json
{
  "status": "success",
  "tx_hash": "<Transaction Hash>",
  "chainId": "<Chain ID>",
  "explorerURL": "<URL to Etherscan/Polygonscan>"
}
```

## API Endpoints

## Table of Contents
- [Company Methods](#company-methods)
- [Token Methods](#token-methods)
- [TGE Methods](#tge-methods)
- [DAO Member Methods](#dao-member-methods)
- [User Methods](#user-methods)
- [Documents Methods](#documents-methods)
- [Invoice Methods](#invoice-methods)
- [Webhooks](#webhooks)


### Company Methods

**GET /available-pools**

Used to get available pools and their prices.

Response:
```json
{
  "pools": "<Array of Available Pools>"
}
```

**POST /pool**

Used to purchase a new pool. 

Request Body:
```json
{
  "poolInfo": "<Pool Info>"
}
```
Response:
```json
{
  "poolAddress": "<Pool Address>"
}
```

**GET /pool/{poolAddress}**

Used to get pool details.

Response:
```json
{
  "poolInfo": "<Pool Info>"
}
```

**GET /user/{userAddress}/pools**

Used to get all pools where the user has a role.

Response:
```json
{
  "pools": "<Array of Pools>"
}
```


**GET /pool/{poolAddress}/events**

Used to get all events associated with a pool.

Response:
```json
{
  "events": "<Array of Events>"
}
```

### Token Methods
**GET /token/{tokenAddress}**

Used to get token details.

Response:
```json
{
  "tokenInfo": "<Token Info>"
}
```

**GET /user/{userAddress}/token/{tokenAddress}/balance**

Used to get user's balance for a specific token created within the platform.

Response:
```json
{
  "balance": "<Token Balance>"
}
```

### TGE Methods
**POST /token/{tokenAddress}/tge**

Used to create a new TGE for a token.

Request Body:
```json
{
  "tgeInfo": "<TGE Info>"
}
```
Response:
```json
{
  "tgeAddress": "<TGE Address>"
}
```

**GET /tge/{tgeAddress}**

Used to get TGE details.

Response:
```json
{
  "tgeInfo": "<TGE Info>"
}
```

### DAO Member Methods
**POST /pool/{poolAddress}/proposal**

Used to create a new proposal.

Request Body:
```json
{
  "proposalInfo": "<Proposal Info>"
}
```
Response:
```json
{
  "proposalId": "<Proposal Id>"
}
```

**POST /pool/{poolAddress}/proposal/{proposalId}/vote**

Used to cast a vote on a proposal.

Request Body:
```json
{
  "vote": "<Vote>"
}
```
Response:
```json
{
  "status": "<Status>"
}
```

**GET /pool/{poolAddress}/proposal/{proposalId}**

Used to get proposal details.

Response:
```json
{
  "proposalInfo": "<Proposal Info>"
}
```

**GET /pool/{poolAddress}/voting-power/{userAddress}**

Used to get user's voting power in DAO.

Response:
```json
{
  "votingPower": "<Voting Power>"
}
```

### User Methods
**GET /user/{userAddress}**

Used to get user details.

Response:
```json
{
  "userInfo": "<User Info>"
}
```

### Documents Methods
**GET /pool/{poolAddress}/documents**

Used to get a list of all documents associated with the pool.

Response:
```json
{
  "documents": "<Array of Documents>"
}
```

**GET /document/{docId}**

Used to get specific document details.

Response:
```json
{
  "documentInfo": "<Document Info>"
}
```

### Invoice Methods
**POST /pool/{poolAddress}/invoice/on-chain**

Used to create a new on-chain invoice.

Request Body:
```json
{
  "invoiceInfo": "<Invoice Info>"
}
```
Response:
```json
{
  "invoiceId": "<Invoice Id>"
}
```

**POST /pool/{poolAddress}/invoice/off-chain**

Used to create a new off-chain invoice.

Request Body:
```json
{
  "invoiceInfo": "<Invoice Info>"
}
```
Response:
```json
{
  "invoiceId": "<Invoice Id>"
}
```

**POST /pool/{poolAddress}/invoice/{invoiceId}/set-paid**

Used to set invoice to paid.

Response:
```json
{
  "status": "<Status>"
}
```

**GET /pool/{poolAddress}/invoice/{invoiceId}**

Used to get invoice details.

Response:
```json
{
  "invoiceInfo": "<Invoice Info>"
}
```

# Webhooks

Webhooks are a way for CompanyDAO.org API to provide real-time information to other applications. They allow you to specify a URL to which we will send HTTP POST requests when certain events occur in the system.

## Setting up Webhooks

To set up a webhook, you need to provide the following information:

- `url`: The URL to which the POST request will be sent when the event occurs. The URL must use HTTPS.
- `events`: The list of events you want to subscribe to.

Webhooks can be set up and managed via the following API endpoints:

**POST /webhooks**

Creates a new webhook.

Request Body:
```json
{
  "url": "<URL>",
  "events": ["<Event1>", "<Event2>", ...]
}
```
Response:
```json
{
  "id": "<Webhook Id>"
}
```

**GET /webhooks**

Gets the list of all webhooks.

Response:
```json
{
  "data": ["<Array of Webhooks>"]
}
```

**GET /webhooks/{webhookId}**

Gets the details of a specific webhook.

Response:
```json
{
  "data": "<Webhook Info>"
}
```

**DELETE /webhooks/{webhookId}**

Deletes a specific webhook.

Response:
```json
{
  "status": "success"
}
```

## Events

The events that can trigger a webhook include:

- `pool.purchased`: Triggered when a new pool is purchased.
- `token.created`: Triggered when a new token is created.
- `tge.created`: Triggered when a new TGE is created.
- `proposal.created`: Triggered when a new proposal is created.
- `vote.cast`: Triggered when a vote is cast on a proposal.
- `invoice.created.onchain`: Triggered when a new on-chain invoice is created.
- `invoice.created.offchain`: Triggered when a new off-chain invoice is created.
- `invoice.paid`: Triggered when an invoice is marked as paid.

When an event occurs, we'll send a POST request to your specified URL with a JSON payload. The payload will include `type`, `data` and `id`.

For example, the payload for a `pool.purchased` event might look like this:

```json
{
  "type": "pool.purchased",
  "data": {
      "object": "<Pool Info>"
  },
  "id": "<Webhook Event ID>"
}
```

Please note, for security reasons, you should verify the POST requests are coming from CompanyDAO.org. 

## Event Retry

If the HTTP POST request fails, CompanyDAO.org will attempt to resend the notification periodically for up to 72 hours. 

## Webhook Signing

To ensure that the webhook requests are indeed coming from CompanyDAO.org, we sign the requests. The signature is included in the header `X-CDAO-Signature`. Your application should verify this signature before processing the request.

## Event Types

Here is a list of event types that can be sent via webhook. 

- `pool.purchased`
- `token.created`
- `tge.created`
- `proposal.created`
- `vote.cast`
- `invoice.created.onchain`
- `invoice.created.offchain`
- `invoice.paid`

