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
{
  "address": "<Relayer Address>",
  "balance": "<Relayer Balance>"
}
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
