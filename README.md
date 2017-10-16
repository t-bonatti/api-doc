# Transfeera API

- [Introdução](#introdução)
- [Fazendo uma requisição](#fazendo-uma-requisição)
- [Apenas JSON](#apenas-json)

# Introdução

A API da Transfeera possibilita que aplicações se comuniquem com a sua conta no sistema. Este documento explica quais os métodos disponíveis para acesso.

# Fazendo uma requisição

A autenticação de todas request é via Bearer token:
Basta adicionar a seguinte propriedade no Header de cada requisição
- Authorization: {token_de_acesso}

Todas as requisições são criptografadas, a Transfeera não aceita requisições feitas com HTTP simples, apenas HTTPS. A URL base da API é https://api.transfeera.com/

Todas as requisições à API da Transfeera devem ser acompanhadas do header User-Agent, use este header para informar qual a sua aplicação e qual o seu email para contato. Veja alguns exemplos de como você pode se identificar usando o header User-Agent:

```
User-Agent: Meu Site (falecom@admin.com.br)
User-Agent: Controle de Estoque (controledeestoque.com.br)
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
  id: #idDoLoteCriado
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
        "received_date": "2017-10-13T16:10:44.000Z",
        "returned_date": null,
        "finish_date": null,
        "number": 4,
        "created_at": "2017-10-13T14:34:01.000Z",
        "payer_name": null,
        "payer_cpf_cnpj": null,
        "transfers_list_report_file": null,
        "user_id": 1
    },
    {
        "id": "623",
        "value": 6.5,
        "status": "AGUARDANDO_RECEBIMENTO",
        "received_date": null,
        "returned_date": null,
        "finish_date": null,
        "number": 9,
        "created_at": "2017-10-16T13:18:40.000Z",
        "payer_name": null,
        "payer_cpf_cnpj": null,
        "transfers_list_report_file": null,
        "user_id": 1
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
```

## Consultar transferência

### Request
`GET /batch/{id}/transfer/{id}`

### Response

```
```

## Consultar transferências dentro de um lote

### Request
`GET /batch/{id}/transfers`

### Response

```
```

## Excluir transferência

### Request
`DELETE /transfer/{id}`

### Response

```
```

## Consultar bancos 
`GET /banks`

### Response

```

```
