## Fluxo de Operação (SmartSindico)

![](https://docs.google.com/drawings/d/sOf3NeFXpAPj6Scnk0FngXg/image?w=624&h=187&rev=197&ac=1&parent=1_rzMBLMC_Wge2aQ22pCAEe_W69Bv0YSKx8DJP5Dv4dM)

### Serviço: `partner.modulo12.com.br`

Todas as URIs abaixo podem ser acessadas através da URL base https://partner.modulo12.com.br
***
# Recurso REST: v1.partner

## Métodos
### Get
`GET	/v1/partner/{partnerId}`

Retorna informações relacionadas ao parceiro. Exemplo: nome, contato técnico, email, etc.

# Recurso REST: v1.partner.customer
## Métodos
### List
`GET	/v1/partner/{partnerId}/customer`

Retorna lista com todos os clientes do parceiro. Cada cliente contém UUID, nome, RG, e CPF.
### Create
`POST	/v1/partner/{partnerId}/customer`

Cria um novo cliente do parceiro. Retorna UUID do cliente criado.

### Get
`GET	/v1/partner/{partnerId}/customer/{customerId}`

Retorna informações sobre cliente do parceiro. Exemplo: Nome, RG, CPF, e dados cadastrais.

### Delete
`DELETE	/v1/partner/{partnerId}/customer/{customerId}`

Deleta cliente do parceiro.

## Recurso REST: v1.partner.customer.transaction

## Métodos
### List
`GET	/v1/partner/{partnerId}/customer/{customerId}/transaction`

Retorna lista de todas operações financeiras pertinentes ao cliente.

### Create
`POST	/v1/partner/{partnerId}/customer/{customerId}/transaction`

Cria uma transação pro cliente. Retorna um UUID que será usado para consultar status da transação.

### Get
`GET	/v1/partner/{partnerId}/customer/{customerId}/transaction/{transactionId}`

Retorna detalhes/status de uma transação. 

### Get (Boleto)
`GET /v1/partner/{partnerId}/customer/{customerId}/transaction/{transactionId}?print_bill=true`

Caso a transação já esteja em status de geração de boleto, esta requisição retorna o arquivo em PDF do boleto relacionado.

### Cancel
`DELETE	/v1/partner/{partnerId}/customer/{customerId}/transaction/{transactionId}`

Cancela uma transação. A transação nunca é deletada da base de dados, somente altera-se o status para cancelada, e cancela qualquer operação manual/automatizada no back-office.

***
