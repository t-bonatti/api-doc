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
