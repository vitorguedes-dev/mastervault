data: 27/10
Objetivo: Organizar as dúvidas que surgiram durante a reunião de apresentação da plataforma de testes e funcionamento do fluxo interno do Pix

---
## GESTOR - Ramael,  Hector e Dani

1. Split de Pagamento -> Repasse de comissões automático entre Tapaggo e Parceiros
	1. Como vai funcionar? E detalhes.
	2. Vai ser um Sistema Próprio? API do BB tem suporte a essa funcionalidade?
	3. Será Gerenciado pelo LIS através de carteira?

2. Contrato Tapaggo x Parceiro
	1. Quais tipos de responsabilidades de cobrança do Financeiro?
		-  Parcelas que estão sendo descontadas, controle de entrada de pagamento, inadimplência, cancelamento e envio de relatório de inadimplência ou plataforma de consulta de cancelamento e inadimplência pelo parceiro. (A inadimplência é de responsabilidade do Parceiro?).
		- Gestão de inadimplência + Cobrança + Contato com o cliente.

3. Gerenciamento de Clientes pelo Financeiro
	1. Os clientes serão separados por parceiro, gerando uma plataforma para cada?
	2. Serão unificados em um única plataforma? 
	3. E serão gerados um split de pagamento para cada pagamento?

4. Verificar custos de tarifas
	1. Para Politica de retentativa e demais serviços

## DESENVOLVEDOR
1. Plataforma de Consulta (Teste) -> Finalizar o teste completo (Adicionar os campos modificáveis de acordo com a API do BB).
		1.1 Formulário de preenchimento do cliente: Quais os dados necessários? Para atender a demanda interna é necessário que os setores Operacional, Financeiro, Relacionamento, etc definam esses dados.
2. Relatório de Inadimplência das cobranças / Tela de acompanhamento de inadimplência das cobranças
3. Verificar se o QRcode gerado é a causa da primeira cobrança ser como Pix comum (Fora do Pix Automático).
	1. Como informado pelo setor financeiro, no extrato bancário o primeiro pagamento aparece no nome do pagador e não do cliente quando o pagamento é realizado por terceiros. Se o pagamento é realizado pelo próprio cliente é indiferente. (Causa Impossibilidade de conciliação bancária entre o extrato e a cobrança recorrente).

	**Possível solução (Caso 3 - Caso seja um comportamento normal da API):** 
	- Verificar se a API retorna os dados do cliente e do pagador.
	- Gerar relatório de cliente e pagador