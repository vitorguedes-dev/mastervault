**data:** 24/10/2025
**Autor:** Vitor

---

Posteriormente, serão feitas atualizações sobre o funcionamento do Pix Automático e a relação do mesmo com o Banco do Brasil, as jornadas especificadas e as integrações
## Esquema das Tabelas de controle da Recorrência e Cobrança

![[Pasted image 20251024112104.png]]

## Tabela rec_slim
Tabela que guarda as informações da Recorrência Criada no sistema do Pix (Banco do Brasil + Sistema Pix do Governo).

**Explicação:** Aqui estão guardadas as informações da recorrência (Tipo da Jornada, Se foi aprovada, se foi encerrada). Não possui informações sobre cobranças realizadas.
### **Descrição de cada campo com exemplo e como usar:**
#### Campos de Identificação internos (Sem uso para usuários finais/clientes)

**id:** Identificação interna
**id_rec:** Identificador único da Recorrência (Provido pelo Banco do Brasil)
**received_at:** Data em que a recorrência foi cadastrada no banco de dados
**updated_at:** Data da última atualização do campo no banco de dados
**ativacao_txid:** Código gerado por solicitação técnica da API do BB
#### Campos para Consulta

**Status:** (CRIADA, APROVADA, CANCELADA) -> Consulta do status da Recorrência (Se o cliente cancelou a autorização de cobrança pelo pix automático).
**ativacao_tipo_jornada**: Tipo da jornada (Até momento Jornada 2 e 3 conforme solicitado no início do desenvolvimento)
**atualizacao:** Histórico de atualizações relacionadas a recorrência com data e status (Por exemplo, momento em que ocorreu a Criação da recorrência e Aprovação ou cancelamento).

Para consultar basta a URL (Endereço) e CHAVE SECRETA (Chave de Acesso) do banco de dados, já informados a Jhennifer e Ramael para conexão com o sistema da CAPEB. 

Para utilização é necessário que o sistema efetue uma busca específica por estes campos utilizando o idRec (Identificador único da recorrência criado pelo Banco do Brasil no momento em que o Cliente autoriza a recorrência). 

**OBS: *Se necessário posso efetuar a criação de um exemplo em código para exemplificar a buscar em termos técnicos.***
## Tabela cobranca_slim
Tabela que guarda os dados gerados pelo pagamento da cobrança recebidos pelo Banco do Brasil.

**Explicação:** Dados relacionados a cobrança em si (Quando o cliente efetua o pagamento de qualquer mensalidade relacionada a recorrência).
### **Descrição de cada campo com exemplo e como usar:**
#### Campos de Identificação internos (Sem uso para usuários finais/clientes)

**id:** Identificação interna
**id_rec:** Identificador único da Recorrência (Provido pelo Banco do Brasil)
**received_at:** Data em que a recorrência foi cadastrada no banco de dados
**updated_at:** Data da última atualização do campo no banco de dados
**txid:** Código gerado por solicitação técnica da API do BB
### Campos para Consulta

**Status:** (CRIADA, ATIVA, CANCELADA, CONCLUIDA) -> Informação sobre a cobrança recorrente, ou seja, informa sobre a situação atual da Cobrança Recorrente na conta do cliente pelo Pix Automático.

- **CRIADA** -> Cliente autorizou a cobrança e a recorrência foi criada, mas ainda não foi efetuado nenhum pagamento.
- **ATIVA** -> Cliente efetuou pelo menos um pagamento de uma cobrança e autorizou a recorrência.
- **CANCELADA** -> Cliente teve uma recorrência autorizada e efetuou o cancelamento no Banco do Brasil
- **CONCLUIDA** -> Cliente efetuou primeiro pagamento e mais um pagamento que estava agendado. Última cobrança recorrente agendada foi paga. 

**rejeicao:** Se houve rejeicao de pagamento da última cobrança.

**DADOS DAS TENTATIVAS DE COBRANÇA:**

- **tentativa_tipo:** Tipo da tentativa (No nosso normalmente AGND -> Agendada)
- **tentativa_status:** 
	- (PAGA, AGENDADA, CANCELADA)  Status da tentativa de cobrança vigente, a depender da data de liquidação da Cobrança, é o status da tentativa de cobrança mais atual.
- **data_liquidacao:** 
	- Data de liquidação da cobrança vigente, ou seja, atualização da cobrança quando atingir essa data. Normalmente 3 ou 4 dias após.

Para utilização é necessário que o sistema efetue uma busca específica por estes campos utilizando o idRec (Identificador único da recorrência criado pelo Banco do Brasil no momento em que o Cliente autoriza a recorrência). 

**OBS: *Se necessário posso efetuar a criação de um exemplo em código para exemplificar a buscar em termos técnicos.***

Como informado por e-mails anteriores a implementação da API do PIX do BB é feita através de jornadas especificadas pelo próprio banco, o que torna necessário a consulta das documentações oficiais sempre que necessário para entender o funcionamento geral da mesma.  

**Atenção: Caso ocorra alguma dúvida em relação ao uso dos fluxos e implementação da API do BB estou à disposição.**