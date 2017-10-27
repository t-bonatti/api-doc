# Introdução

- [Introdução](#introdução)
- [Fazendo uma requisição](#fazendo-uma-requisição)
- [Utilização da API via JSON](#utilização-da-api-via-json)
- [Status dos lotes](#status-dos-lotes)
- [Status das transferências](#status-das-transferências)
- [Contas bancárias da Transfeera](#contas-bancárias-da-transfeera)

# API

- [Tipo de contas suportados](#tipo-de-contas-suportados)
- [Criar lote sem nenhuma transferência](#criar-lote-sem-nenhuma-transferência)
- [Criar lote com transferências](#criar-lote-com-transferências)
- [Editar lote](#editar-lote)
- [Consultar lote](#consultar-lote)
- [Fechar lote](#fechar-lote)
- [Excluir lote](#excluir-lote)
- [Consultar lotes](#consultar-lotes)
- [Criar transferência](#criar-transferência)
- [Consultar transferência](#consultar-transferência)
- [Consultar transferências dentro de um lote](#consultar-transferências-dentro-de-um-lote)
- [Excluir transferência](#excluir-transferência)
- [Consultar bancos](#consultar-bancos)
- [Consultar favorecidos](#consultar-favorecidos)


# Introdução

A API da Transfeera possibilita que aplicações externas se comuniquem com a sua conta na plataforma Transfeera. Este documento explica quais os métodos disponíveis para acesso.

# Fazendo uma requisição

A autenticação das requisições é realizada via token, paara ter acesso ao token siga as instruções abaixo:


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


# Utilização da API via JSON

A API só suporta JSON, nós não vamos dar suporte a outro formato. Mesmo que você não utilize o header ```Content-Type: application/json; charset=utf-8``` a resposta será em JSON e com charset utf-8.

# Status dos lotes
- AGUARDANDO_RECEBIMENTO: Aguardando receber o valor total do lote na conta bancária da Transfeera
- RECEBIDO: Identificamos o pagamento do lote na nossa conta bancária
- FINALIZADO: Efetuamos todas as transferências e enviamos o relatório
- REMOVIDO: Lote removido
- RASCUNHO: Lote em rascunho aceita a inserção de novas transferências até ser fechado
- DEVOLVIDO: O valor total do lote, ou seja, a soma de todas as transferências, foi devolvido para o usuário


# Status das transferências
- CRIADA: Criada com sucesso
- RECEBIDO: Identificamos o pagamento da transferência na nossa conta bancária
- TRANSFERIDO: Transferência efetuada porém ainda sem comprovante bancário linkado
- FINALIZADO: Transferência efetuada e comprovante bancário recebido
- REMOVIDO: Removido
- FALHA: Tentamos efetuar o pagamento, porém, por motivos de falha dos dados do favorecido ou falha na comunicação com o internet banking a transferência não foi efetuada.

# Contas bancárias da Transfeera

## Banco do Brasil
- Agência: 5214-0
- Conta: 13568-2

## Santander
- Agência: 0159 
- Conta: 13006828-3

## Bradesco
- Agência: 2232
- Conta: 40605-8

## Itaú
- Agência: 8842
- Conta: 47600-7

## Caixa Econômica
- Agência: 3282
- Conta: 1172-7

## Sicoob
- Agência: 3039
- Conta: 62277-0

# Tipo de contas suportados
- CONTA_CORRENTE: Conta corrente
- CONTA_POUPANCA: Conta poupança
- CAIXA_FACIL: Conta caixa fácil

# Criar lote sem nenhuma transferência

## Request
`POST /batch`

### Body
`Precisa ser enviado um objeto vazio.`
```
{
}
```

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
          "account_type": "CONTA_CORRENTE"
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
    "payer_name": "Fulano souza",
    "payer_cpf_cnpj": "73677801400",
    "transfers": [
       {
        "id": 5483, #caso queira editar uma transferência dentro do lote
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
          "account_type": "CONTA_CORRENTE"
        }
       },
       { 
        ## sem ID vai criar uma nova transferência
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
    "name": "Fulano de souza",
    "cpf_cnpj": "12863843893",
    "bank_id": 1,
    "email": "",
    "agency": "3333",
    "agency_digit": "0",
    "account": "12345",
    "account_digit": "1",
    "account_type": "CONTA_CORRENTE"
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
            "name": "Fulano de souza",
            "cpf_cnpj": "12863843893",
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
`DELETE /batch/{id}/transfer/{id}`

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

