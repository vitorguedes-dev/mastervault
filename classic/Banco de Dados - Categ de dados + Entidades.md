Data: 29/09/2025
Autor: Vitor Hugo
Objetivo: Definir o básico baseado nos sistemas atuais integrados e categorizar por entidades

---
## Necessidade Urgente

Devido ao aumento de complexidade, integração do LIS, Site e Cielo. Surgiu a necessidade de tabelas mapeadas e com relacionamentos bem definidos para comportar a lógica necessária de acompanhamento do pagamento do link gerado. 

Dessa forma, continua sendo necessário a resposta das perguntas feitas no documento de **Resumo das Necessidades Urgentes** com foco nos sistemas essenciais pra venda dos produtos e principalmente do sistema Lis visto que é o sistema fim a ser utilizado em todas as integrações.

Segue seleção das necessidades retiradas do documento supracitado:

- **Modelagem Do Banco**
	- Tabelas e relacionamento
	- Quais os dados que serão armazenados?
	- De quais fontes eles virão?
	- Qual banco deve ser usado em qual caso?
- **Implementação**
	- Efetivamente construir o banco de de dados e integrá-lo 
	- Integrar com APIs
	- Integrar com a Plataforma (Site, CRM, Lis, Cielo, N8N, Sistemas Parceiros)

O documento citado pode ser encontrado em anexo.

***Reforço que há a necessidade de uma consultoria especializada para lidar com a construção da diagramação e implementação do banco de dados devido a grande complexidade, inclusive, pensando nos sistemas parceiros que estão sendo desenvolvidos para que se encaixe em uma nova proposta de arquitetura de infraestrutura.***

Abaixo segue um **mapeamento básico** dos dados necessários identificados até agora:

**OBS: esses dados foram identificados como necessários ao decorrer das integrações. Deve ser realizada uma análise em conjunto com a equipe de dados e desenvolvimento com foco em mapear todos os dados a serem incluídos no Banco e como eles estarão relacionados.**
## Entidade Lead

### Site (Planos/Assistências)
- id_lead
- Nome
- CPF
- Número de celular
- Data de Nascimento
- E-mail
- Endereço Completo (CEP, UF, Cidade, Bairro, Rua, complemento)
- Gênero
- Pets
	- Quantidade
	- Nomes?
- Método de Pagamento
- Dados do Pagador: Nome e CPF (Se houver)
- Nome do Produto
### CRM
- id_crm
- Dados básicos (Nome, celular, CPF, Gênero, etc...)
- Descrição
- Valor Total dos Produtos Simulados
- Renda estimada
- Valor por Produto
- Status
- Source
- Endereço
- Dados do Pagador: Nome e CPF (Se houver)
- Quantidade de planos simulados por produto
- Data da simulação
- Tags (Produtos)
## Entidade Cliente
- id
- id_lead
- id_lis
- id_crm
- Dados Básicos
- checkout_cielo
- order_number
- produtos
- valor total dos produtos comprados
- genero
- id_contrato
## Entidade Produto
- id
- cod_lis
- nome
- planos
- coberturas
## Entidade Transação (controle de compras)
- id
- cod_produto
- id_cliente
- id_lead
- id_crm
- cpf_cliente
- cpf_pagador
- nome_pagador
- email_pagador
- telefone_pagador
- amount
- checkout_cielo
- payment_order
- payment_status
- payment_method
## Entidade Contrato (Padrão Lis)
- id
- id_lis (número do contrato gerado pelo lis)
- cpf_cnpj
- titular
- produto
- plano
- corretor
- codigo_vendedor
- inicio_vigencia
- fim_vigencia
- frequencia_emissao
- parcelamento
- email
- telefone
- forma_pagamento
- permite_email
- permite_sms
- permite_whatsapp

***Ainda faltam os dados que serão obtidos através do formulário de simule agora do site, visto que possui uma estrutura diferente do site e dados diferentes além do que aguarda uma atualização para inclusão de dados essenciais como os beneficiários do titular*.**

### 1. **Planejamento e Descoberta (Semanas 1-2)**

**Objetivo:** Alinhar requisitos e entender os fluxos.

**Tarefas:**

- [ ]  Reunião com stakeholders (Site, CRM, LIS, Cielo, Parceiros).
- [ ]  **Mapear casos de uso e cenários ponta a ponta.**
- [ ]  Definir escopo inicial de entidades (Lead, Cliente, Produto, Transação, Contrato).
- [ ]  Validar fontes de dados com equipes.

**Entregáveis:**

- Documento de Requisitos Prioritários V1.
- Fluxograma de integrações.
- **Lista de Casos de Uso e Cenários (nível alto).**

---

### 2. **Modelagem de Dados & Arquitetura (Semanas 3-5)**

**Objetivo:** Estruturar o banco de dados.

**Tarefas:**

- [ ]  Criar Diagrama ER (Entidade-Relacionamento). - Consultor
- [ ]  Definir chaves e relacionamentos. - Consultor
- [ ]  Estruturar dicionário de dados. - Consultor
- [ ]  Escolher tecnologia de banco. - Consultor
- [ ]  **Consultoria especializada para validação arquitetural.**

**Entregáveis:**

- Documento Modelagem Banco V1 (ERD + Dicionário de Dados).
- Proposta de infraestrutura.

---

### 3. **Implementação Técnica (Semanas 6-10)**

**Objetivo:** Construir o banco e integrar sistemas.

**Tarefas:**

- [ ]  Criar scripts de DDL (tabelas).
- [ ]  Configurar ambientes do banco.
- [ ]  Integrar Site, CRM, LIS, Cielo com o novo banco.
- [ ]  Implementar orquestração via N8N.

**Entregáveis:**

- Banco de dados funcional (homologado).
- APIs documentadas e testadas.

---

### 4. **Validação e Testes (Semanas 11-12)**

**Objetivo:** Garantir consistência e estabilidade.

**Tarefas:**

- [ ]  Criar e executar cenários de testes (baseados nos casos de uso).
- [ ]  Validar consistência dos dados.
- [ ]  Ajustar erros.
- [ ]  Documentação técnica final.

**Entregáveis:**

- Relatório de testes.
- Banco validado em produção.
- Manual de documentação.

---

### 🔑 **Checklist de Recursos Necessários**

- Equipe de Dados.
- Equipe de Integradores.
- Documentação APIs (LIS, Cielo, CRM, Site).
- **Consultoria especializada em arquitetura de dados e integração.**
- Ambiente de infraestrutura.

---
### 📝 **Esboço de Casos de Uso e Cenários (Exemplos)**

#### **Entidade: Lead**

- **Caso de Uso:** Captura de Lead pelo Site
    - **Cenário 1 (Sucesso):** Usuário preenche formulário no Site, Lead é criado no banco e enviado ao CRM.
    - **Cenário 2 (Erro):** Falha na comunicação com CRM, Lead é armazenado e retentativa agendada.

#### **Entidade: Cliente**

- **Caso de Uso:** Conversão de Lead em Cliente
    - **Cenário 1 (Sucesso):** Lead realiza compra via Site/CRM, pagamento aprovado, Cliente é criado/atualizado no banco, contrato gerado no LIS.
    - **Cenário 2 (Erro):** Pagamento recusado, Cliente não é criado/atualizado, status de transação registrado.

#### **Entidade: Transação**

- **Caso de Uso:** Processamento de Pagamento
    - **Cenário 1 (Sucesso):** Cliente efetua pagamento via Cielo, Transação é registrada como "aprovada".
    - **Cenário 2 (Erro):** Pagamento falha/recusado, Transação é registrada como "recusada", notificação ao cliente.

#### **Entidade: Contrato**

- **Caso de Uso:** Geração e Gestão de Contrato
    - **Cenário 1 (Sucesso):** Após pagamento aprovado, LIS gera contrato, dados do contrato são registrados no banco.
    - **Cenário 2 (Erro):** LIS indisponível, contrato fica em status "pendente de geração", retentativa agendada.