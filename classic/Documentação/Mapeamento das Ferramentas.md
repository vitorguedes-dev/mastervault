Autor: Vitor
Data: 18/08/2025
Objetivo: Mapeamento Inicial da estrutura do ecossistema da plataforma

---
## Sistemas Principais

### Star - Attime
Sistema Legado, base de dados em migração para o Lis.
### Lis - CS Prime
Sistema de BPO Operacional, irá gerenciar as seguintes atividades da Master:

- Movimentações de Segurados (Cancelamento, Falecimento, Inclusões, Exclusões)
- Emissão de Certificados Automatizados
- Gerenciamento de Informações do segurado (Documentos, beneficiários, contratos, planos)
- Apenas novas contratações são cadastradas.
- Envia os dados do cliente quando solicitado.
### Site
Venda dos seguros e assistências da masterClassic;

- Venda online do seguro de vida e assistências
- Envia os dados de novos cadastros (Leads)
- Envia os dados de novos clientes (Contratações)
- Envia os dados primários de pagamento (Geração do Link)
### CRM
Sistema que gerencia a parte de acompanhamento do cliente nas etapas de Atendimento, Geração do Link de Pagamento, Pós Venda, Cadastro.

- Gerenciamento dos Leads (Possível Cliente)
- Gerenciamento das Tarefas da equipe (Substituiu Trello)
- Automações básicas
- Gerenciamento de Projetos
### Alter Data
Sistema que gerencia a parte do setor Financeiro da masterClassic.

- Pagamentos
- Cobranças
- Provisionamento em bancos
- Recebimento
-  Comprovantes dessas Transações

---
## Serviços e Plataformas

### Google Drive
Arquivamento, Backup e compartilhamento.
### Gmail
Comunicação interna, com parceiros e clientes (Depende do Setor)

- Atendimento ao cliente (Solicitações de Certificado)
- Comunicação externa (Parceiros, Seguradoras, Estipulantes)
- Comunicação interna (Entre setores e colaboradores)
- Solicitações internas (Certificado, Carteira, Movimentações)
### Sonax Chat
Gerenciamento do atendimento ao cliente

- Atendimento de clientes - Por escrito e Ligação
- Pós Venda
- Atendimento de casos de Sinistro
- Solicitações de Certificado
### MantisBT - Attime
Abertura de chamados relacionados ao STAR.
### Serviços da TeraInfo
- Suporte em T.I
- Armazenamento de Rede (Drive interno)
- Configuração de Infra e hardware

---
## Ferramentas de Infraestrutura

### Portainer
Gerenciamento de containers Docker, multi cluster e kubernetes

- Stacks de serviços conteinerizados (Redis, Postgres, n8n...)
- Maior controle dos conteiners docker -> Exige conhecimento avançado em docker para gerenciamento manual
- Monitoramento das stacks (Uso de memória, CPU, rede)
### Oriondesign
Ferramenta de terminal usada para instalação automática de serviços e configurada como portainer, zapbot, typebot, n8n, etc 
### Hetzner
Serviço de Cloud contratado para a VPS (Virtual Private Server) da masterclassic. 

- Mantém todos os nossos serviços e plataformas utilizados.
- Provém os recursos de hardware necessários virtualizados em nuvem.
- Provém o IP de acesso atrelado ao DNS e domínio da master.
### Supabase
Serviço de gerenciamento de Banco de Dados em nuvem

- Mantém o banco de dados inicial atual da masterclassic
- Integra com N8N
- Dados dos leads e clientes
### Termius
Gerenciamento do servidor e SFTP.

- Conexão com os servidores da VPS da Hetzner
- Acesso aos arquivos dos servidores via SFTP
- Acesso ao terminal dos servidores (SO: Ubuntu)
## Ferramentas de Desenvolvimento

### N8N
Automações e BackEnd low code.

- Gerenciamento de requisições (CRUD) ao CRM, Supabase, Lis.
- Cadastro de Clientes e Leads no CRM, Lis e Supabase.
- Gerenciamento de Estado de transações (Pagamento e Geração de Link na Cielo).
- Gerenciamento de Requisições do Typebot e Site (Contratação, Cadastro, etc).
### Typebot
Automação de atendimento ao cliente.

- Fluxos do chatbot do site
- Redirecionamento de links de pagamento (CIELO) e Landing pages
- Envio de informações ao N8N (Ativação de Webhook).

### Wordpress + Elementor
**CMS** (sistema de gestão de conteúdo) **open-source** usado para criar sites e blogs. É feito em **PHP** e usa **MySQL/MariaDB**.

- Ferramenta em que o site foi criado
- Permite algumas edições em PHP
### Zapbot
Automações relacionadas ao atendimento via Whatsapp

- É ponto de redirecionamento do zapbot e N8N
- API que é utilizada para envio de mensagens ao whatsapp
- Possui automações específicas relacionadas a isso.
### PHP + HTML, CSS e JS

- PHP: usado para edição dos formulários do site.
- JS: Usado para funções específicas no N8N (Gerar Payloads, manipulação de dados).
- HTML/CSS: Poucas alterações no site.