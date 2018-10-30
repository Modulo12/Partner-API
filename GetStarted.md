Primeiros passos
===================

### Introdução
----------

Este tutorial tem como objetivo apresentar exemplos de como utilizar a API de parceiros do Banco Maré. 


### Como fazer uma requisição
As informações da API são disponibilizadas por meio de *webservices* utilizando arquitetura [REST] ( Representational State Transfer). 

Todos os *endpoints* da plataforma possuem a mesma **URL Base** (temporária):

>  - **URL:** http://partner.modulo12.com.br

O detalhamento de cada método disponível nas API's pode ser encontrada na [referência](/Reference.md).

#### Exemplo
Como exemplo, executaremos um fluxo simples que vai desde a criação do parceiro até a geração de transações. Para começar, é necessário criar um registro de parceiro na plataforma. Esse registro futuramente poder ser criado previamente pelo Banco Maré para possíveis parceiros, porém atualmente está aberto para cadastro aberto pelo período de testes da API.

##### 1 - Cadastro de Partner
 > **POST**  -  `http://partner.modulo12.com.br/v1/partner/`
 
Para cadastro das informações referentes ao parceiro, devem ser adicionados no corpo da requisição obrigatoriamente os atributos de email, nome e contato técnico. O campo opcional "info" é um atributo dinâmico que pode tomar forma de qualquer tipo de JSON necessário. No exemplo abaixo, são utilizados atributos de teste apenas para visualização.

**Request Body:**
```json
 {
  "contatoTecnico": "(61) 3107-4189",
  "email": "contato@modulo12.com.br",
  "info": {
    "info1": "teste1",
    "info2": "teste2"
  },
  "nome": "Módulo12"
}
```

**Response:** 201 (Created). A informação sobre o registro criado é retornada por meio do header **Location**. Como pode ser observado abaixo, o recurso foi criado com sucesso e sua UUID é **1**.

**Response Headers**:
```json
{
  "location": "http://partner.modulo12.com.br/v1/partner/1",
  "date": "Tue, 30 Oct 2018 13:46:03 GMT",
  "content-length": "0",
  "content-type": null
}
```

##### 2 - Cadastro de Customer

 > **POST**  -  `http://partner.modulo12.com.br/v1/partner/{partnerId}/customer`
 
 Tendo o parceiro seu cadastro completado na plataforma, ele então pode cadastrar seus clientes informando CPF, nome e a UUID do parceiro relacionado (neste caso, a que criamos no último passo). Novamente o campo JSON dinâmico "info" pode ser utilizado da forma que o parceiro desejar, inserindo as informações que forem necessárias para seu cliente.

**Request Body:**
```json

{
  "cpf": "04520320301",
  "info": {
    "info3": "teste3"
  },
  "nome": "Cliente 1",
  "partner": 1
}

```

**Response:** 201 (Created). A informação sobre o registro criado é retornada por meio do header **Location**. Como pode ser observado abaixo, o recurso foi criado com sucesso e sua UUID é **2**.

**Response Headers**:
```json
{
  "location": "http://partner.modulo12.com.br/v1/partner/1/customer/2",
  "date": "Tue, 30 Oct 2018 13:59:03 GMT",
  "content-length": "0",
  "content-type": null
}
```

