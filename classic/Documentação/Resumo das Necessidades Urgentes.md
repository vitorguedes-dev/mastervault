
Autor: Vitor
Data: 13/08/2025
Motivo: Resumir as necessidades do projeto

---

Devido a contratação em andamento este é um resumo das melhorias propostas que vai ser realizada para os alicerces da plataforma masterClassic. Lembrando que isso é uma generalização para entendimento geral. **Reforço que são sugestões de acordo com minhas considerações.**

--- 
## Profissionais Necessários
Cada parte descrita na página inicial do documento de melhoria proposto é realizado por um profissional de T.I de área específica.
### Arquiteto de Software/Soluções/TechLead
Responsável por atuar na definição de toda a arquitetura da infraestrutura do sistema, exemplo, definição de como será a escalabilidade, técnicas para mitigar gargalos, elasticidade e coesão dos dados trafegados de acordo com a volumetria de requisições e clientes. Definir a Stack (tecnologias que vão ser usadas no projeto)
### DevOps
Responsável por implementar a infraestrutura (Servidores, ferramentas de gerenciamento de containers, ferramentas de cloud),  versionamento, separação de ambientes e tecnologias usadas para tal.
### DBA/Analista de BD
Responsável pela modelagem, diagramação e arquitetura do banco de dados.
### Profissional em Metodologia Ágil / Processos de dev de software
Definição de toda a parte organizacional, documentação e de processos, fluxos, diagramas, regras de negócio (em conjunto com Techlead), metas e objetivos.
### Desenvolvedor/Programador
Implementar a estrutura do software, códigos, automações, tratamento de erros. Outras responsabilidades dependem da área específica (FrontEnd, Backend, etc).

---
## Por que?
Todo sistema cresce  conforme o escopo que pretende atender e as bases precisam ser definidas para que o crescimento até o escopo desejado não seja impedido ou o sistema falhe em atender o escopo atual no meio do processo. 
#### Quer dizer que precisa ser uma super estrutura de início? Não
Mas quer dizer que a estrutura inicial precisa ser definida para que o crescimento não tenha gargalos. Então, precisamos ter o escopo a ser buscado, o escopo atual, os objetivos do sistema, a arquitetura atual mapeada para podermos definir uma arquitetura escalável e elástica.
#### Quer dizer que precisamos de todos esses profissionais agora?
Seria bom, mas acredito que não. É possível mapear todas as tecnologias usadas hoje e definir as tecnologias e arquitetura que usaremos. Com o sistema funcional,  podemos realizar as melhorias necessárias para alcançar esse objetivo. Mas primeiro isso tem que ser definido.
## Tarefas Urgentes

1. **Mapeamento da Arquitetura Atual** (Desde as tecnologias até os fluxos de cada etapa do processo do cliente, dos processos de automação, dos processos de servidor).
2. **Definição clara do escopo da plataforma e o que vai ser entregue/atendido agora.**
3. **Definição do fluxo de trabalho dos envolvidos** (Estrutura dos cards de tarefas, definição das tarefas, definição de concluído, definição do andamento da tarefa).
4. **Definição do foco e dos objetivos de curto/médio prazo.**
5. **Definição de urgência das solicitações.**
### Banco de dados
Os sistemas de banco de dados são tecnologias feitas para garantir estabilidade, segurança e confiança, portanto, não são facilmente mutáveis. Dessa forma, é imprescindível a modelagem do banco antes da criação. Pois, é possível que gere muitos problemas de escalabilidade e disponibilidade do serviço oferecido pela master e até mesmo perca de informações. 
#### O que é necessário?
- Modelagem do banco
	1. Onde esses dados serão usados?
	2. Para que fim os dados serão usados?
	3. Quais os dados que serão armazenados?
	4. De quais fontes eles virão?
	5. Quais os tipos computacionais desses dados?
	6. Qual a volumetria desses dados?
	7. Quais dados serão buscados com frequência?
	Tudo isso serve de base para definirmos o tamanho, quantos bancos de dados serão necessários, a finalidade de cada um deles (Write, Read, Deep Archive, elástico) e como serão dispostos na Arquitetura do Sistema. Após isso, deve ser feita a diagramação das tabelas,
	8. Diagrama de Entidade-Relacionamento, Diagramas lógico e conceitual

- Definição da estrutura
	1. Qual banco deve ser usado em qual caso? (Postgres, AWS, MySQL)
	2. Qual o tamanho inicial?
	3. Como irá interagir com os serviços?
	4. Auto hospedado, físico ou nuvem?
	5. Sistemas de Cache?

- Implementação do Banco de Dados
	1. Efetivamente construir o banco de de dados e integrar com as outras soluções
	2. Integrar com APIs
	3. Integrar com a Platafoma (Site, CRM, Backend?, N8N)
	4. Construir as tabelas (Scripts SQL?). Depende da infraestrutura definida*

Estas são dúvidas que eu acho válido serem consideradas nesse processo dado meu pouco conhecimento e experiência de como é um sistema web dessa magnitude e o que eu julgo ser o objetivo da master com esta plataforma (Atender a milhares ou milhão de usuários no Brasil todo e também atender a Parceiros, empresas tão grandes quanto a master).