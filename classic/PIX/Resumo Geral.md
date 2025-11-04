Resumo geral das atividades e reuniões! ***Arquivos relacionados em anexo.***
### Automações
Foram realizadas as automações do site integrando cadastro e update com Cielo, LIS, CRM e banco de dados juntamente com a atualização do banco depois do upload de bases da luyara no CRM.

Foi criado modelo de importação dos dados no CRM.

Foi feito backup manual dos fluxos do N8N e Typebot.

**Em andamento:**
- Reformulação fluxos para eficiência, documentação, correção de erros e estruturação dos dados
- Criação do fluxo do Pix automático com conexão com a API do BB (Não consta no Kanban)
- Reformulação dos fluxos de integração com LIS.
- Reformulação dos fluxos de integração com Cielo
- Reformulação e estruturação dos dados do Typebot e Site

**Necessário:**
- Definir a prioridade das tarefas

*OBS: Na segunda-feira, após reunião na qual foi informado a necessidade do uso do Kanban, da definição de escopo dos sistemas e definição de prioridades das tarefas e projetos, foi explicitado que o sistema do pix automático estava pronto e só recebi a documentação e a necessidade da criação da automação na Sexta-Feira.*
### Ferramentas e Sistemas

Foram mapeados todos os sistemas e ferramentas em uso atualmente e suas funções.

Foi criado um bot para testes dos formulários no site.

Foram mapeadas todas as ferramentas de desenvolvimento necessárias, já foram instaladas pela Attime a pedido do Wender.
### Arquitetura, Documentação e Processos
Foram realizadas a definição da estrutura do Kanban e dos cards das tarefas visando a organização e a melhora do fluxo de trabalho da equipe em conjunto com Pablo, adaptando o uso da metodologia ao nosso cenário.

Foi proposta a arquitetura da infraestrutura baseado nos sistemas que estavam em vigência e levando em consideração um fluxo baixo de tráfego devido a não definição do escopo.

Foi proposta a Stack de desenvolvimento utilizando o framework NextJS explicando as vantagens e desvantagens em relação as tecnologias usadas no momento levando em consideração as nossas necessidades e sistemas construídos.

Foi proposto 2 formas de separação de ambientes, em conversa com Ramael, foi decidido que será necessário a criação de um servidor especifico para ambiente de Desenvolvimento e Stagging/Homologação/Testes. 

Foi definido o uso do Supabase como banco de dados devido ao Lis não permitir a criação de produtos via API e considerações acerca de complexidade levando em conta a equipe atual, limitações e custos financeiros.

Foi criado o plano de implementação do versionamento de código com a ferramenta Git, juntamente com as convenções, para trabalho em colaboração mútua, controle de alterações e backup.

**Em andamento:**
- Criação da arquitetura geral do sistema (decisões acerca da infraestrutura e padrões de arquitetura levando em consideração a volumetria de acessos e clientes) - Aguardando a definição de escopo, requisitos e objetivos da plataforma.
- Criação dos padrões de arquitetura a serem usados na arquitetura do código.
- Construção do documento de requisitos funcionais e não funcionais em conjunto com Pablo
- **Demais tarefas a acompanhar no Kanban**
### Banco de Dados
Após definido que o supabase será usado como banco de dados a ser utilizado para manter os dados em tráfego na plataforma, é necessário que seja definido o escopo dos sistemas para que seja avaliado questões de performance e estratégia na gerência dos dados em relação aos diversos sistemas que utilizarão o mesmo.

**Em andamento:**
- Estruturação dos dados obrigatórios e básicos dos clientes e leads que trafegam entre os sistemas para garantir a estabilidade e integridade dos dados
- Diagramação e montagem do banco de dados em conjunto com Wender  -> necessário decidir sobre o escopo da plataforma para que a tarefa seja executada em sua completude
### Considerações
Existem tarefas em teste e aguardando feedback no quadro e tarefas que precisam de definição de prioridade. Assim como, tarefas que envolvem o sistema de Gestão que está sendo construído que precisa entrar no processo de uso do Kanban e precisa de definição de escopo para que entre na análise de arquitetura geral, infra e padrão de uso do banco de dados.

