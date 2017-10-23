# Transfeera API

- [Introdução](#introdução)
- [Fazendo uma requisição](#fazendo-uma-requisição)
- [Apenas JSON](#apenas-json)

# Introdução

A API da Transfeera possibilita que aplicações se comuniquem com a sua conta no sistema. Este documento explica quais os métodos disponíveis para acesso.

# Fazendo uma requisição

A autenticação de todas request é via token, para ter acesso ao token siga as instruções abaixo:
Faça um request passando seu login e senha do seu usuário cadastrado no Transfeera

### Request
`POST https://api.transfeera.com/authenticate`

### Form data

```
{
  login: <seuEmail>
  password: <suaSenha>
}
```

### Response

```
{
  token: 'seuTokenDeAcesso'
}
```

Agora com o token basta adicionar a seguinte propriedade no Header de cada requisição
- Authorization: {token_de_acesso}

Todas as requisições são criptografadas, a Transfeera não aceita requisições feitas com HTTP simples, apenas HTTPS. A URL base da API é https://api.transfeera.com/

Todas as requisições à API da Transfeera devem ser acompanhadas do header User-Agent, use este header para informar qual a sua aplicação e qual o seu email para contato. Veja alguns exemplos de como você pode se identificar usando o header User-Agent:

```
User-Agent: Sua empresa (contato@suaempresa.com.br)
```


# Apenas JSON

A API só suporta JSON, nós não vamos dar suporte a outro formato. Mesmo que você não utilize o header ```Content-Type: application/json; charset=utf-8``` a resposta será em JSON e com charset utf-8.


# Criar lote sem nenhuma transferência

## Request
`POST /batch`

## Response

```
{
  id: #idDoLoteCriado
}
```

# Criar lote com transferências
### Request
`POST /batch`

### Body

```
{
    transfers: [
       {
        "value": 25.0,
        "integration_id": 2,
        "destination_bank_account": {
          "name": "Fernando Nunes 2",
          "cpf_cnpj": "08910914912",
          "bank_id": 1,
          "email": "",
          "agency": "3299",
          "agency_digit": "0",
          "account": "16232",
          "account_digit": "7",
          "account_type": "CONTA_CORRENTE"
        }
       },
       { 
        "value": 25.0,
        "integration_id": 2,
        "destination_bank_account": {
          "name": "Fernando Nunes 2",
          "cpf_cnpj": "08910914912",
          "bank_id": 1,
          "email": "",
          "agency": "3299",
          "agency_digit": "0",
          "account": "16232",
          "account_digit": "7",
          "account_type": "CONTA_CORRENTE"
        }
       }
    ]
}
```

### Response

```
{
  id: #idDoLoteCriado
}
```

# Editar lote
### Request
`PUT /batch/{id}`

### Body

```
{
    transfers: [
       {
        "id": 5483, #caso queira editar uma transferência dentro do lote
        "value": 25.0,
        "integration_id": 2,
        "destination_bank_account": {
          "name": "Fernando Nunes 2",
          "cpf_cnpj": "08910914912",
          "bank_id": 1,
          "email": "",
          "agency": "3299",
          "agency_digit": "0",
          "account": "16232",
          "account_digit": "7",
          "account_type": "CONTA_CORRENTE"
        }
       },
       { 
        ## sem ID vai criar uma nova transferência
        "value": 25.0,
        "integration_id": 2,
        "destination_bank_account": {
          "name": "Fernando Nunes 2",
          "cpf_cnpj": "08910914912",
          "bank_id": 1,
          "email": "",
          "agency": "3299",
          "agency_digit": "0",
          "account": "16232",
          "account_digit": "7",
          "account_type": "CONTA_CORRENTE"
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
  number: 2,
  created_at: "2017-10-13T14:30:24.000Z",
  payer_name: null,
  payer_cpf_cnpj: null,
}
```

## Consultar lote

### Request
`GET /batch/{id}`

### Response

```
{
    "id": "617",
    "value": 5,
    "status": "REMOVIDO",
    "number": 3,
    "payer_name": null,
    "payer_cpf_cnpj": null,
    "received_date": null,
    "returned_date": null,
    "finish_date": null,
    "created_at": "2017-10-13T14:30:24.000Z"
}
```

## Fechar lote

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

## Excluir lote

### Request
`DELETE /batch/{id}`

## Consultar lotes

### Request
`GET /batch`

### Response

```
[
    {
        "id": "618",
        "value": 5,
        "status": "RECEBIDO",
        "number": 4,
        "payer_name": null,
        "payer_cpf_cnpj": null,
        "created_at": "2017-10-13T14:34:01.000Z"
    },
    {
        "id": "623",
        "value": 6.5,
        "status": "AGUARDANDO_RECEBIMENTO",
        "number": 9,
        "payer_name": null,
        "payer_cpf_cnpj": null,
        "created_at": "2017-10-16T13:18:40.000Z"
    }
]
```

## Criar transferência

### Request
`POST /batch/{id}/transfer`

### Body

```
{
  "value": 25.0,
  "integration_id": 2,
  "destination_bank_account": {
    "name": "Fernando Nunes 2",
    "cpf_cnpj": "08910914912",
    "bank_id": 1,
    "email": "",
    "agency": "3299",
    "agency_digit": "0",
    "account": "16232",
    "account_digit": "7",
    "account_type": "CONTA_CORRENTE"
  }
}
```

### Response

```
{
    "id": "5589",
    "value": 25,
    "status": "CRIADA",
    "destination_bank_account_id": 5029,
    "batch_id": "617"
}
```

## Consultar transferência

### Request
`GET /transfer/{id}`

### Response

```
{
    "id": "5490",
    "value": 5,
    "status": "RECEBIDO",
    "integration_id": null,
    "created_at": "2017-10-13T14:34:01.000Z",
    "destination_bank_account_id": 5027,
    "receipt_url": null,
    "bank_receipt_url": null
}
```

## Consultar transferências dentro de um lote

### Request
`GET /batch/{id}/transfer`

### Response

```
[
    {
        "id": "5490",
        "value": 5,
        "status": "RECEBIDO",
        "created_at": "2017-10-13T14:34:01.000Z",
        "integration_id": null,
        "DestinationBankAccount": {
            "id": 5027,
            "name": "Rafael Negherbon",
            "cpf_cnpj": "08910914912",
            "bank_id": 1,
            "email": null,
            "agency": "123",
            "agency_digit": null,
            "account": "123",
            "account_digit": "1",
            "account_type": "CONTA_CORRENTE"
        },
        "receipt_url": null,
        "bank_receipt_url": null
    }
]
```

## Excluir transferência

### Request
`DELETE /transfer/{id}`

## Consultar bancos 
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

## Consultar favorecidos 
`GET /destinationBankAccount`

### Response

```
[
    {
        "id": 5028,
        "name": "Favorecido teste",
        "account": "16232",
        "account_digit": "6",
        "agency": "3299",
        "agency_digit": null,
        "cpf_cnpj": "08910914912",
        "email": null,
        "bank_id": 3,
        "created_at": "2017-10-16T15:16:58.000Z"
    }
]
```

