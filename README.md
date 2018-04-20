# Introduction

- [Introduction](#introduction)
- [Authentication](#authentication)
- [Using the API with JSON](#using-the-api-with-json)
- [Batch statuses](#batch-statuses)
- [Transfer statuses](#transfer-statuses)
- [Our bank accounts](#our-bank-accounts)

# API

- [Supported accounts](#supported-accounts)
- [Create batch without any transfer](#create-batch-without-any-transfer)
- [Create batch with transfers](#create-batch-with-transfers)
- [Edit batch](#edit-batch)
- [Get batch data](#get-batch-data)
- [Close batch](#close-batch)
- [Remove batch](#remove-batch)
- [Get batches data](#get-batches-data)
- [Create transfer](#create-transfer)
- [Get transfer data](#get-transfer-data)
- [Get transfer data inside a batch](#get-transfer-data-inside-a-batch)
- [Remove transfer](#remove-transfer)
- [Get banks data](#get-banks-data)
- [Get recipient data](#get-recipient-data)
- [Get recipient data that has payments](#get-recipient-data-that-has-payments)


# Introduction

The Transfeera API enables external applications to communicate with your account on our platform. This document explains the methods that are available.

# Authentication

The authentication of the requests is performed using a token, to enable access to the API and generate a Token, please contact us by email contato@transfeera.com.

When we enable the API for your user, access our platform, and in the menu with your name click on "Segurança" (Security), and in the "API" section generate a token.

With the token you obtained, on every request to our API include a header with the token like this:
```
Authorization: {token}
```

All our requests are encrypted, Transfeera does not accept requests made with simple HTTP, only HTTPS. The base URL to the API is https://api.transfeera.com/.

All requests to the Transfeera API must be sent with the User-Agent header. Use this header to tell us which application and email address to contact. Here is an example of how you can identify yourself using the User-Agent header:

```
User-Agent: Company (contact@company.com)
```


# Using the API with JSON

The API only supports JSON, we will not support another format. Even if you do not use the `Content-Type: application / json; charset = utf-8` the response will be in JSON and with charset utf-8.

# Batch statuses
- AGUARDANDO_RECEBIMENTO: Awaiting to receive the total amount of the batch in our bank account
- RECEBIDO: We have identified the payment of the batch
- FINALIZADO: We have made all transfers and sent the report
- REMOVIDO: Removed batch
- RASCUNHO: Batch in draft accepts new transfers until closed
- DEVOLVIDO: The total amount of the batch, that is, the sum of all transfers, was refunded to the user


# Transfer statuses
- CRIADA: Successfully created
- RECEBIDO: We have identified the payment for the transfer in our bank account
- TRANSFERIDO: We have made the transfer, but still without bank receipt
- FINALIZADO: We have made the transfer, and the bank receipt is available
- REMOVIDO: Removed
- FALHA: We tried to make the payment, however, due to incorrect data or failure to communicate with the internet banking, the transfer was not made

# Our bank accounts

## Banco do Brasil
- Agency: 5214-0
- Account: 13568-2

## Santander
- Agency: 0159
- Account: 13006828-3

## Bradesco
- Agency: 2232
- Account: 40605-8

## Itaú
- Agency: 8842
- Account: 47600-7

## Caixa Econômica
- Agency: 3282
- Account: 1172-7

## Sicoob
- Agency: 3039
- Account: 62277-0

# Supported accounts
- CONTA_CORRENTE: Current account
- CONTA_POUPANCA: Savings account
- CAIXA_FACIL: "caixa fácil" account

## Create batch without any transfer

### Request
`POST /batch`

### Body
`Required to send an empty object.`
```
{
}
```

### Response

```
{
  id: #createdBatchId
}
```

## Create batch with transfers
### Request
`POST /batch`

### Body

```
{
    "name": "",
    "payer_name": "",
    "payer_cpf_cnpj": "",
    "transfers": [
       {
        "value": 25.0,
        "integration_id": 2,
        "destination_bank_account": {
          "name": "Fulano de souza",
          "cpf_cnpj": "12863843893",
          "bank_id": 1,
          "email": "",
          "agency": "3333",
          "agency_digit": "0",
          "account": "12345",
          "account_digit": "1",
          "account_type": "CONTA_CORRENTE" #CONTA_CORRENTE || CONTA_POUPANCA || CAIXA_FACIL
        }
       },
       {
        "value": 25.0,
        "integration_id": 2,
        "destination_bank_account": {
          "name": "Fulano de souza",
          "cpf_cnpj": "12863843893",
          "bank_id": 1,
          "email": "",
          "agency": "3333",
          "agency_digit": "0",
          "account": "12345",
          "account_digit": "1",
          "account_type": "CONTA_CORRENTE" #CONTA_CORRENTE || CONTA_POUPANCA || CAIXA_FACIL
        }
       }
    ]
}
```

### Response

```
{
  id: #createdBatchId
}
```

## Edit batch
### Request
`PUT /batch/{id}`

### Body

```
{
    "name": "",
    "payer_name": "Fulano souza",
    "payer_cpf_cnpj": "73677801400",
    "transfers": [
       {
        "id": 5483, #if you want to edit a transfer within the batch
        "value": 25.0,
        "integration_id": 2,
        "destination_bank_account": {
          "name": "Fulano de souza",
          "cpf_cnpj": "12863843893",
          "bank_id": 1,
          "email": "",
          "agency": "3333",
          "agency_digit": "0",
          "account": "12345",
          "account_digit": "1",
          "account_type": "CONTA_CORRENTE" #CONTA_CORRENTE || CONTA_POUPANCA || CAIXA_FACIL
        }
       },
       {
        ## without ID will create a new transfer
        "value": 25.0,
        "integration_id": 2,
        "destination_bank_account": {
          "name": "Fulano de souza",
          "cpf_cnpj": "12863843893",
          "bank_id": 1,
          "email": "",
          "agency": "3333",
          "agency_digit": "0",
          "account": "12345",
          "account_digit": "1",
          "account_type": "CONTA_CORRENTE" #CONTA_CORRENTE || CONTA_POUPANCA || CAIXA_FACIL
        }
       }
    ]
}
```

### Response

```
{
  id: 10,
  value: 50.0,
  name: "Lote 2",
  created_at: "2017-10-13T14:30:24.000Z",
  payer_name: null,
  payer_cpf_cnpj: null,
}
```

## Get batch data

### Request
`GET /batch/{id}`

### Response

```
{
    "id": "617",
    "value": 5,
    "status": "REMOVIDO",
    "name": "Lote 3",
    "payer_name": null,
    "payer_cpf_cnpj": null,
    "received_date": null,
    "returned_date": null,
    "finish_date": null,
    "created_at": "2017-10-13T14:30:24.000Z"
}
```

## Close batch

### Request
`POST /batch/{id}/close`

### Response
```
{
    "name": "Santander",
    "agency": "0159",
    "account": "13006828-3",
    "company": "Transfeera Serviços de Pagamento LTDA.",
    "cnpj": "27.084.098/0001-69"
}
```

## Remove batch

### Request
`DELETE /batch/{id}`

## Get batches data

### Request
`GET /batch?endDate=2018-04-30&initialDate=2018-04-01&page=0`

### Response

```
[
    {
        "id": "618",
        "value": 5,
        "status": "RECEBIDO",
        "name": "Lote 4",
        "payer_name": null,
        "payer_cpf_cnpj": null,
        "created_at": "2017-10-13T14:34:01.000Z"
    },
    {
        "id": "623",
        "value": 6.5,
        "status": "AGUARDANDO_RECEBIMENTO",
        "name": "Lote 9",
        "payer_name": null,
        "payer_cpf_cnpj": null,
        "created_at": "2017-10-16T13:18:40.000Z"
    }
]
```

## Create transfer

### Request
`POST /batch/{id}/transfer`

### Body

```
{
  "value": 25.0,
  "integration_id": 2,
  "destination_bank_account": {
    "name": "Fulano de souza",
    "cpf_cnpj": "12863843893",
    "bank_id": 1,
    "email": "",
    "agency": "3333",
    "agency_digit": "0",
    "account": "12345",
    "account_digit": "1",
    "account_type": "CONTA_CORRENTE" #CONTA_CORRENTE || CONTA_POUPANCA || CAIXA_FACIL
  }
}
```

### Response

```
{
    "id": "7198",
    "value": 25,
    "status": "CRIADA",
    "destination_bank_account_id": "5479",
    "batch_id": "793",
    "integration_id": "2"
}
```

## Get transfer data

### Request
`GET /transfer/{id}`

### Response

```
{
    "id": "5490",
    "value": 5,
    "status": "RECEBIDO",
    "status_description": null, # only filled when transfer fail
    "integration_id": null,
    "created_at": "2017-10-13T14:34:01.000Z",
    "destination_bank_account_id": 5027,
    "receipt_url": null,
    "bank_receipt_url": null
}
```

## Get transfer data inside a batch

### Request
`GET /batch/{id}/transfer`

### Response

```
[
    {
        "id": "5490",
        "value": 5,
        "status": "RECEBIDO",
        "status_description": null, # only filled when transfer fail
        "created_at": "2017-10-13T14:34:01.000Z",
        "integration_id": null,
        "DestinationBankAccount": {
            "id": 5027,
            "name": "Fulano de souza",
            "cpf_cnpj": "12863843893",
            "bank_id": 1,
            "email": null,
            "agency": "123",
            "agency_digit": null,
            "account": "123",
            "account_digit": "1",
            "account_type": "CONTA_CORRENTE" #CONTA_CORRENTE || CONTA_POUPANCA || CAIXA_FACIL,
            "Bank": {
              "name": "Banco do Brasil"
            }
        },
        "receipt_url": null,
        "bank_receipt_url": null
    }
]
```

## Remove transfer

### Request
`DELETE /batch/{id}/transfer/{id}`

## Get banks data
`GET /bank`

### Response

```
[
    {
        "id": 5,
        "name": "Itaú"
    },
    {
        "id": 1,
        "name": "Santander"
    },
    {
        "id": 4,
        "name": "Bradesco"
    },
    {
        "id": 6,
        "name": "Banco do Brasil"
    },
    {
        "id": 7,
        "name": "Sicoob"
    },
    {
        "id": 3,
        "name": "Caixa Econômica"
    }
]
```

## Get recipient data
`GET /destinationBankAccount`

### Response

```
[
    {
        "id": 5028,
        "name": "Fulano de souza",
        "account": "1234",
        "account_digit": "1",
        "agency": "3299",
        "agency_digit": null,
        "cpf_cnpj": "12863843893",
        "email": null,
        "bank_id": 3,
        "created_at": "2017-10-16T15:16:58.000Z"
    }
]
```

## Get recipient data that has payments
`GET /destinationBankAccount/hasPayments/{cpfCnpj}`

### Response

```
{
    "id": "123",
    "name": "Fulano de tal",
    "cpf_cnpj": "67312962289",
    "agency": "123",
    "agency_digit": "1",
    "account": "321",
    "account_digit": "1",
    "bank_id": "4"
}
```
