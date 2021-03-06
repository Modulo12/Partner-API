Primeiros passos
===================

### Como fazer uma requisição
As informações da API são disponibilizadas por meio de *webservices* utilizando arquitetura [REST] ( Representational State Transfer). 

Todos os *endpoints* da plataforma possuem a mesma **URL Base** (temporária):

>  - **URL:** http://partner.modulo12.com.br

O detalhamento de cada método disponível nas API's pode ser encontrada na [referência](/Reference.md).

#### Exemplo
Como exemplo, executaremos um fluxo simples que vai desde a criação do parceiro até a geração de transações. Para começar, é necessário criar um registro de parceiro na plataforma. Esse registro futuramente poder ser criado previamente pelo Banco Maré para possíveis parceiros, porém atualmente está aberto para cadastro aberto pelo período de testes da API.

##### 1 - Cadastro de Partner
 > **POST**  -  `http://partner.modulo12.com.br/v1/partner/`
 
Para cadastro das informações referentes ao parceiro, devem ser adicionados no corpo da requisição obrigatoriamente os atributos de email, nome e contato técnico. O campo opcional "info" é um atributo dinâmico que pode tomar forma de qualquer tipo de JSON necessário. No exemplo abaixo, são utilizados atributos de teste apenas para visualização. Esse campo JSON dinâmico deve ser passado como uma String de JSON escapado ([escaped JSON](https://www.baeldung.com/java-json-escaping)) utilizando o caracter '\' antes das aspas duplas internas. 

**Request Body:**
```json
 {
  "contact": "(61) 3107-4189",
  "email": "contato@modulo12.com.br",
  "info": "{\"info1\": \"teste1\",\"info2\": \"teste2\"}",
  "name": "Módulo12"
}
```

**Response:** 201 (Created). A informação sobre o registro criado é retornada por meio do header **Location**. Como pode ser observado no exemplo abaixo, o recurso foi criado com sucesso e sua UUID é **1**.

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
 
 Tendo o parceiro seu cadastro completado na plataforma, ele então pode cadastrar seus clientes informando CPF, nome e a UUID do parceiro relacionado (neste caso, a que criamos no último passo). Novamente o campo JSON dinâmico "info" pode ser utilizado da forma que o parceiro desejar, inserindo as informações que forem necessárias para seu cliente. Esse campo JSON dinâmico deve ser passado como uma String de JSON escapado ([escaped JSON](https://www.baeldung.com/java-json-escaping)) utilizando o caracter '\' antes das aspas duplas internas. 

**Request Body:**
```json

{
  "cpf": "04520320301",
  "info": "{\"info1\": \"teste1\"}",
  "name": "Cliente 1"
}

```

**Response:** 201 (Created). A informação sobre o registro criado é retornada por meio do header **Location**. Como pode ser observado no exemplo abaixo, o recurso foi criado com sucesso e sua UUID é **2**.

**Response Headers**:
```json
{
  "location": "http://partner.modulo12.com.br/v1/partner/1/customer/2",
  "date": "Tue, 30 Oct 2018 13:59:03 GMT",
  "content-length": "0",
  "content-type": null
}
```

##### 3 - Criação de Transaction

 > **POST**  -  `http://partner.modulo12.com.br/v1/partner/{partnerId}/customer/{customerId}/transaction`
 
Para criar uma nova transação, é necessário informar via Request Body, obrigatoriamente, os seguintes campos:

**Request Body:**
```json

{
   "dueDate": "2018-05-21",
  "tenantAddress": {
    "bairro": "string",
    "cep": "string",
    "cidade": "string",
    "logradouro": "string",
    "uf": "string"
  },
  "tenantBirthDate": "1980-01-10",
  "tenantCellPhone": "61-900000000",
  "tenantCpf": "string",
  "tenantEmail": "test@email.com",
  "tenantFirstName": "string",
  "tenantLastName": "string",
  "type": 0,
  "value": 0
}

```

**Response:** 201 (Created). A informação sobre o registro criado é retornada por meio do header **Location**. Como pode ser observado abaixo, o recurso foi criado com sucesso e sua UUID é **3**.

**Response Headers**:
```json
{
  "location": "http://partner.modulo12.com.br/v1/partner/1/customer/2/transaction/3",
  "date": "Tue, 30 Oct 2018 13:59:03 GMT",
  "content-length": "0",
  "content-type": null
}
```
##### 4 - Acessar status da Transaction

 > **GET**  - `http://partner.modulo12.com.br/v1/partner/{partnerId}/customer/{customerId}/transaction/{transactionId}/status`
 
Acessa o status atual da transação. A resposta traz o código do status assim como sua descrição.

**Response:** 200 (OK).

**Response Body**:
```json
{
  "statusCode": 1,
  "statusDescription": "Pending"
}
```

##### 4 - Obter informações/boleto da Transaction

 > **GET**  -  `http://partner.modulo12.com.br/v1/partner/{partnerId}/customer/{customerId}/transaction/{transactionId}?print_bill=true`
 
 Esta operação retorna as informações referentes à transação solicitada, e caso a variável "print_bill" seja enviada como *request parameter* com o valor **true** e seu status seja **02** (Boleto confirmado pelo SICOOB) o retorno é um arquivo em PDF contendo o boleto referente à esta transação.

**Response:** 200 (OK).

**Response Body (print_bill=false)**:
```json

{
  "id": 8,
  "tenantFirstName": null,
  "tenantLastName": null,
  "tenantBirthDate": null,
  "tenantEmail": null,
  "tenantCellPhone": null,
  "tenantCpf": "02125532085",
  "statusCode": 2,
  "statusDescription": "Entrada Confirmada",
  "type": 0,
  "value": 15,
  "timestamp": "2019-01-29T19:33:08.000+0000",
  "dueDate": "2019-01-29",
  "customer": 91,
  "tenantAddress": {
    "logradouro": "CLN 114",
    "bairro": "Asa Norte",
    "cep": "70764540",
    "cidade": "Brasilia",
    "uf": "DF"
  },
  "info": null
}
```

**Response Body (print_bill=true)**:

Arquivo PDF contendo o boleto.

