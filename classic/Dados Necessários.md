
## Dados Obrigatórios | Cliente

### Dados Pessoais - Obrigatório
- Nome -> Titular
- Estado Civil
- Gênero
- dt_nasc
- CPF
- Telefone -> 14 caracteres
- E-mail
- CEP
- Endereço completo -> Rua, Bairro, complemento, Cidade, Estado
## Dados do Lead
- Dados pessoais +
- id_crm
- produtos
- status -> 
- source -> fonte do lead
- descrição -> informações adicionais
- dados da simulação
## Dados Lis
-  contrato / id_lis
### Dados da contratação
- **Forma de pagamento**
	- Dados bancários de acordo com a forma (Recebido da Cielo)
	- Dia do Vencimento
	- Anual/Mensal
- **Produtos contratados/simulados**
	- Faixa de Renda
	- Inicio da Vigência (dt da contratação)
	- **Seguro de Vida**
		- Capital
		- Prêmio
		- Coberturas
		- Planos
		- Beneficiários
	- **Assistências**
		- Produto
		- Plano
		- Prêmio
			- **Pet (Ocasional)**
				- Nome dos Pets
				- Dt_nasc
				- Peso
				- Raça
				- Cachorro ou Gato
## Dados Após o Pagamento - Transação
- **CIELO**
	- order_number -> identificador
	- amount -> valor
	- checkout_cielo_order_number -> identificador
	- payment_method_type -> forma de pagamento
	- payment_status -> Status do pagamento
	- customer_identity -> CPF
	- product_description -> informações sobre a contratação
### Dados de Classificação e categorização
