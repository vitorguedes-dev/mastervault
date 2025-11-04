**Autor:** Vitor
**Data:** 26/08/2025
**Objetivo:** Definir estratégia de versionamento dos códigos dos sistemas atuais para facilitação do deploy na nova arquitetura de infraestrutura.

---
## Atual
Atualmente, o deploy e gerenciamento de versão é manual, sendo o deploy feito via SFTP no termius ou local.
## Git
**Referência de comandos no GIT:** [Cheatsheet | Lista de comandos](chrome-extension://efaidnbmnnnibpcajpcglclefindmkaj/https://about.gitlab.com/images/press/git-cheat-sheet.pdf)

A ferramenta GIT foi inventada por Linus Torvalds (mesmo criador do Linux) para gerenciar as mudanças feitas e adicionadas durante o desenvolvimento do kernel do linux e se tornou uma ferramenta essencial em qualquer desenvolvimento que envolva programação.

Assim que as ferramentas do ambiente de desenvolvimento forem instaladas, os projetos deverão ser migrados para o mesmo.

- Criar os repositórios remotos no GitHub na conta da masterclassic
- Fazer download dos arquivos do servidor, 
- organizar a estrutura de pastas de acordo com a arquitetura por features
- Realizar o commit inicial com a versão atual do projeto

Será considerado o estado atual de todos os projetos, evitar o esforço em organizar as versões antigas.
## Conventional Commit
Referência rápida: [Artigo do Medium](https://medium.com/linkapi-solutions/conventional-commits-pattern-3778d1a1e657)
A convenção de mensagens nos commits de cada alteração, iremos usar inicialmente os padrões:

	Essa convenção é usada amplamente em qualquer ambiente de desenvolvimento relacionado a programação.

- `feat`: indica o desenvolvimento de uma nova _feature a_o projeto. **Exemplo:** Acréscimo de um serviço, funcionalidade, _endpoint_, etc.
- `refactor`: usado quando houver uma refatoração de código que não tenha qualquer tipo de impacto na lógica/regras de negócio do sistema. **Exemplo:** Mudanças de código após um _code review_
- `style`: empregado quando há mudanças de formatação e estilo do código que **não alteram** o sistema de nenhuma forma.  
    **Exemplo:** Mudar o _style-guide_, mudar de convenção _lint_, arrumar indentações, remover espaços em brancos, remover comentários, etc..
- `fix`: utilizado quando há correção de erros que estão gerando bugs no sistema.  
    **Exemplo:** Aplicar tratativa para uma função que não está tendo o comportamento esperado e retornando erro.
- `chore`: indica mudanças no projeto que não afetem o sistema ou arquivos de testes. São mudanças de desenvolvimento.  
    **Exemplo:** Mudar regras do _eslint_, adicionar _prettier_, adicionar mais extensões de arquivos ao ._gitignore_
- `docs`: usado quando há mudanças na documentação do projeto.  
    **Exemplo:** adicionar informações na documentação da API, mudar o _README_, etc.
- `build`: utilizada para indicar mudanças que afetam o processo de build do projeto ou dependências externas.  
    **Exemplo:** _Gulp_, adicionar/remover dependências do _npm_, etc.
- `revert`: indica a reverão de um _commit_ anterior.
### Branches
Branches são ramificações das alterações, segue exemplo:

- **Main ->** código final que está em produção, deployado no servidor, código em produção
- **Stagging ->** Código que está finalizado em teste, será a fonte do deploy do servidor de testes.
- **Dev ->** Código que está sendo desenvolvido constantemente.

Deverá ser avaliado a criação de outras branches a medida que os projetos crescerem.
### Estrutura de pastas
Referências rápidas: [Documentação do NextJS](https://nextjs.org/docs/app/getting-started/project-structure)
[Exemplo de Feature-based](https://www.tabnews.com.br/m4c1el/a-melhor-estrutura-de-pastas-pro-seu-projeto-next-js-feature-based-folder-structure)

**Será implementada para projetos novos ou que forem refatorados (revisados).**
Temos duas opções, não excludentes entre si:

- Estrutura de organização por feature -> as pastas representam funcionalidades
- Estrutura proposta pelo NextJs -> as pastas seguem a lógica proposta pelo framework

Deve se levar em consideração que as pastas no Nextjs também são usadas para definir as rotas na aplicação.
### Github
Github é a plataforma de repositórios Git mais usada no mundo, cerca de 90% de todos os códigos estão hospedados no mesmo. 

- Criar perfil no github (Quem não tiver)
- Criar conta da masterClassic
- Entrar como contribuidor nos projetos da masterClassic
ou
- Entrar como parte do time da masterClassic (Se a conta da masterClassic for de empresa).
### SemVer
Estratégia de Semantic Version (Versionamento Semântico), torna as versões das alterações significativas para mapear a possibilidade de "voltar no tempo", ou seja, reverter as alterações para um ponto em que o sistema funcionava em caso de quebra ou bugs rapidamente:

**Formato:** vX.Y.Z -> v1.0.0:
**Significado:** X = Major (Versões completas) | Y = Minor (Funcionalidades novas) | Z = Patch (Correções pontuais)

**OBS:** Será implementado quando for possível ou após a primeira versão da aplicação desenvolvida.

