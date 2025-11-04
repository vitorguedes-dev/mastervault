data: 03/10/2025
Autor: Vitor Hugo

---

Bom dia,

Segue resumo do desenvolvimento dos sistemas desempenhado até o momento:

Como informado anteriormente, durante o desenvolvimento da Integração com LIS, foram identificados diversos pontos que precisavam de revisão. Foi avaliado que era necessário refazer a jornada de cadastro para que fosse possível a integração.

Existem duas categorias de produtos os planos (Assistências) e o Seguro de Vida. Para cada caso será necessário formas diferentes de integração devido ao Seguro de Vida e os planos acoplados estarem atrelados ao Sistema de Produtos. 

A etapa de cadastro dos PLANOS foi dividida em 3 partes, segue:

- **Cadastro Básico - Lead (Site, CRM, Banco de dados):**
	- Categorização dos dados necessários da Etapa
	- Cadastro de Lead
		- Cadastro no Banco de dados (Supabase)
		- Cadastro no CRM
		- Atualização no Banco de dados 
		- Atualização do CRM
- **Cadastro Intermediário - Link de Pagamento Gerado (Site, Banco de dados, CRM, CIELO)**
	- Categorização dos dados necessários (CIELO, LIS, Produto)
	- Atualização do Lead
		- Atualização no Banco de dados
		- Atualização no CRM
- **Cadastro Fim - Cliente gera o pagamento na CIELO (CIELO, LIS, Site, Banco de dados)**
	- Integração da Cielo - Dados da Transação + Status
		- Dados da Transação (Cliente preenche os dados para pagamento)
			- Identifica se é Cliente ou terceiro pagador.
			- Atualização do Banco de dados
			- Atualização do CRM
			- Cadastro da Transação
		- Status da Transação - Controle de Alterações da Transação
			- Identifica se foi pago
			- Atualização da Transação
			- Atualização do banco de dados

**Dessa forma, este é o desenvolvimento até o momento resultando em:**
- Organização dos dados e reformulação dos sistemas
- Identificação de pagamento de terceiros
- Atualização do CRM de acordo com as necessidades informadas
- Integração modular com a CIELO

**Os próximos passos:**
- Integração com LIS - Cadastro Externo e Interno
- Atualização dos Sistemas com as informações geradas pelo LIS
- Refazer o cadastro do Seguro de Vida e planos acoplados
	- Reformulação do Formulário Simular atendendo as demandas da área técnica
	- Análise do Sistema de Produtos
	- Criação dos fluxos de integração
- Criação de Links de Pagamento via API CIELO
- Criação dos fluxos de consulta a CIELO

**OBS: As mudanças apontadas só entrarão em vigor quando a integração com Lis estiver funcional e testada para evitar inconsistências. Reforço que todas soluções desenvolvidas necessitam de testes e análise para ajustes futuros por se tratar de versões funcionais**

**Desenvolvimento Pontual:**
- Foi criado um fluxo para receber **informações sobre o PIX automático (Quando o cliente efetua o pagamento ou autoriza a cobrança)** mas é necessário informar qual sistema irá receber essas informações (URL), em anexo consta um documento com as informações que são recebidas pela API do BB.

**Necessidade Atuais:**
- É necessário o mapeamento dos dados e a definição do banco de dados a ser usado para modelagem do mesmo, considerando todos os sistemas atuais e de parceiros.
- Após isso, é necessário reformular a infraestrutura atual para atender a volumetria de dados e de usuários possíveis, bem como a quantidade de acessos que vão ser necessários.

Aguardem novas informações acerca de outros projetos, qualquer dúvida estou à disposição! Segue links para acompanhamento básico da integração:  
  
Fluxograma: 

https://www.tldraw.com/f/jOlyMA-3tIVDTr0ScUYVU?d=v3554.48.3359.1853.Jq7Xa45CtdE0xqss5Iwmo