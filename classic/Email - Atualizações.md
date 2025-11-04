Integração do LIS

Bom dia,  
  
Iniciando a integração da plataforma com o LIS, foram identificados diversos pontos de mudança nos sistemas do site. (Mudanças e atualizações necessárias foram apontadas em reunião anterior).  
  
O sistema atual não tem suporte a integração do LIS devido às seguintes situações:  
  
- Não solicitamos o endereço dos clientes.
- Os produtos do site não tem uma base sólida para busca de informações que seja possível associar ao LIS.
- Os produtos do formulário SIMULAR (Seguro de vida + Planos Agregados) são baseados no sistema de produtos que não se atualiza conforme as mudanças no LIS. (Avaliar situação)
- Atualização do pagamento inconsistente.
	- Uma segurada pagou no nome de outra pessoa e os dados foram perdidos.
- Fluxos com dados inconsistentes, com muitas responsabilidades acopladas, jornada de cadastro inconsistente que dificulta integrações futuras.

O cadastro no LIS via API exige dados do titular, endereço completo, CNAE, produto, plano e coberturas (Apenas no seguro de vida), os quais não estão mapeados na jornada de contratação.

***OBS: Existem mais dificuldades específicas encontradas que também devem ser revisadas.***

Dessa forma, é necessário realizar a reestruturação da jornada de cadastro e atualização completa. Sugiro que todas as integrações devem ser revistas para suportar futuras integrações e garantia de estabilidade do que temos atualmente (Site, Typebot, N8N, Sistemas de Produtos, Supabase, CRM), 

Proponho que a jornada de cadastro seja separada em 3 etapas distintas, Cadastro de Leads, Atualização do cliente quando gerar o Link, atualização do cliente quando pagamento for efetuado. Mapeando todos os gargalos conhecidos em cada etapa e visando o escopo de integração com o LIS. 

***OBS: Devido a quantidade de particularidades em cada etapa, é importante construir a modelagem das tabelas do banco de dados.***

**Ações a serem realizadas:**
- Criação de sub-workflows para cada ação
- Estruturação dos dados trafegados desde o site até o Lis
- Fluxos que tratam as particularidades de negócio (Exemplo: Desconto, Qtd de Pets, pagamento de terceiros)
- Atualização automática de produtos cadastrados no Lis com os demais sistemas
- Reestruturação do Typebot para enviar os dados necessários ao webhook correto do n8n.

**Já temos:**
- Fluxo de cadastro de planos no Lis. Versão inicial
- Fluxo de cadastro de seguro de vida no Lis. Versão inicial
- Tabela de produtos no Supabase cadastrados no Lis com seus respectivos planos e coberturas. Exceto produtos 18 e 19 (Minha família segura e minha casa segura) por erro na busca da API, foi informado ao Lukinhas.

**É possível acompanhar parte do progresso de mapeamento e fluxogramas via links abaixo:**

https://www.tldraw.com/f/jOlyMA-3tIVDTr0ScUYVU?d=v-117.-110.2292.1264.Jq7Xa45CtdE0xqss5Iwmo

https://www.tldraw.com/f/4S4OOIm6Hov5tBBVZNbUx?d=v-422.-185.2547.1405.ivzeO9aGPivSOTOfsjkrF