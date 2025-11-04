Data: 16/10/2025
Objetivo: Resumo das tarefas urgentes

---
### PIX
 - Controle do Status da recorrência gerada.
 - Finalização da implementação completa da API do BB.
	 - Consulta, edição, criação de recorrência,
 - Verificar possibilidade de integração com o Lis (LIS não possui suporte ao Pix).
 - Estudo sobre Split de Pagamento no Banco do Brasil.
### Servidor
- Criação do servidor com Coolify e infraestrutura preparada com N8N queue, redis e demais ferramentas
	- Postgres, Credenciais do N8N, Redis e demais ferramentas
- Criação de fluxos de backup no Drive e carregamento automático direto do Drive.
### Geral
-  Replicação da solução criada para integração do Site, CRM, LIS, Supabase.
	- Replicação dos fluxos do Typebot
	- Modificação do atual para atender o Plano PET
	- Publicação dos Typebots dentro do Wordpress
	- Execução dos testes para garantia de funcionamento
- Passar os processos pra Maria.
	- Emissão -> Certificados, documentos e cancelamentos
	- Carteira -> Carteira pessoa física, jurídica e repasses
### Sistema de Gestão
- Implementação das necessidades do setor técnico.
	- Analise do documento criado pelo Bruno.
	- Definição dos requisitos funcionais e não funcionais (Regras de negócio) do sistema
	- Implementação
- Revisão das integrações do sistema
- Definição das funcionalidades a serem implementadas em cada parte.

---
## Descrição dos fluxos funcionais

**Primeiro:** Os fluxos que estão sendo utilizados pela Plataforma da Classic ainda são os presentes na Orkai.
	- Devido as configurações necessárias (Autenticação, urls, Tokens) que devem ser aplicadas quando o fluxo muda de servidor.

### Fluxos - Servidor da Orkai
#### PASTA - PIX - Produção
-  **PIX - Send e PIX - Receiver** -> Um recebe uma notificação da recorrência com os dados da recorrência criada para consulta. PIX - Send enviaria essas informações para algum endpoint a ser definido.
- Todos os fluxos de Jornada (INSURTECH e padrão) estão funcionais conforme informado nos e-mails.
#### PASTA - CADASTRO, LIS e demais pastas
Os fluxos nestas pastas estão sendo desenvolvidos no Servidor da Master

**Os fluxos sem pastas estão com a tag [MASTER-CLASSIC] estão sendo usados em produção pela plataforma até o momento.**

### Fluxos - Servidor da Master

**OBS: neste servidor os fluxos estão sendo desenvolvidos de forma integrada.** 
#### PASTA - Pix Produção
- Os fluxos nesta pasta só precisam ser configurados de acordo, para entrar em funcionamento.
#### PASTA - Lis
- Fluxos que lidam com o cadastro direto no Lis
	- Validações de dados
	- Definição de padrões
	- Controle de Contratos
	- Atualização do Banco de dados
**PASTA - Cadastro**
- Fluxos que lidam com  a etapa de Cadastro no CRM e Banco de dados. Organizam os dados para o cadastro no Lis
	- PASTA CIELO - Integração Cielo -> Controle de transações e atualizações das mesmas
	- Cadastro de leads e Cadastro de cliente
	- Todas atualizações das etapas no Banco de Dados
#### Fluxos sem pasta
Neste servidor são apenas Backup.

**OBS: Todos os fluxos nas pastas estão sendo usados mesmo que estejam desativados, eles representam uma lógica interna que não tem necessidade de integração externa ou recebimento de webhooks.**