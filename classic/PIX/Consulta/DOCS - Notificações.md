# Documentação da API: Pix Automático - Webhook Cobrança (Agendamento)

**Objetivo:** Gerenciamento e notificação de cobranças recorrentes (agendamentos) via Webhook.

**Versão da Especificação:** 07/2025 (Spec Bacen 2.8.2)

## 1. Configurações de Conexão

|                            |                                            |                          |                                |
| -------------------------- | ------------------------------------------ | ------------------------ | ------------------------------ |
| **Ambiente**               | **URL Base**                               | **Header Authorization** | **Query Param gw-dev-app-key** |
| **Homologação (sem mTLS)** | `https://api.extranet.hm.bb.com.br/pix/v2` | `Bearer seu Token`       | `suaAppKeyDeHomologacao`       |
| **Homologação (com mTLS)** | `https://api-pix.hm.bb.com.br/pix/v2`      | `Bearer seu Token`       | `suaAppKeyDeHomologacao`       |
| **Produção (com mTLS)**    | `https://api-pix.bb.com.br/pix/v2`         | `Bearer seu Token`       | `suaAppKeyDeProducao`          |
| **Header Fixo**            | `Content-Type: application/json`           | -                        | -                              |

## 2. Endpoints da API

### 2.1. Configurar Webhook (Cadastramento/Ativação)

|                |            |                     |                                                                                                                      |
| -------------- | ---------- | ------------------- | -------------------------------------------------------------------------------------------------------------------- |
| **Recurso**    | **Método** | **Escopo**          | **Descrição**                                                                                                        |
| `/webhookcobr` | `PUT`      | `webhookcobr.write` | Envia a URL para onde as notificações de cobranças recorrentes (associadas ao CNPJ do recebedor) devem ser enviadas. |

#### Request Body (PUT /webhookcobr)

|   |   |   |   |   |
|---|---|---|---|---|
|**Campo**|**Tipo**|**Obrigatório**|**Limite**|**Exemplo**|
|`webhookUrl`|`string <uri>`|S|$\le 140$ caracteres|`https://usuario.recebedor.com/api/webhookcobr/`|

#### Respostas (PUT /webhookcobr)

|   |   |
|---|---|
|**Status**|**Descrição**|
|**200**|Configuração e ativação do serviço bem-sucedida.|
|**400**|Requisição com formato inválido.|
|**403**|Violação de regra de autorização.|
|**404**|Recurso não encontrado.|
|**503**|Serviço indisponível.|

### 2.2. Consultar Webhook Cadastrado

|                |            |                    |                                                                                        |
| -------------- | ---------- | ------------------ | -------------------------------------------------------------------------------------- |
| **Recurso**    | **Método** | **Escopo**         | **Descrição**                                                                          |
| `/webhookcobr` | `GET`      | `webhookcobr.read` | Retorna os dados de configuração do webhookcobr associado ao usuário recebedor (CNPJ). |

#### Respostas (GET /webhookcobr)

|   |   |   |   |
|---|---|---|---|
|**Status**|**Campo (JSON Body)**|**Tipo**|**Descrição**|
|**200**|`webhookUrl`|`string <uri>`|URL configurada.|
|**200**|`criacao`|`string <date-time>`|Data e hora em que o webhook foi cadastrado (ReadOnly).|
|**404**|-|-|Webhook não configurado/ativado.|

### 2.3. Cancelar Webhook

|   |   |   |   |
|---|---|---|---|
|**Recurso**|**Método**|**Escopo**|**Descrição**|
|`/webhookcobr`|`DELETE`|`webhookcobr.write`|Cancela o cadastramento do webhook de cobranças recorrentes.|

#### Respostas (DELETE /webhookcobr)

|   |   |
|---|---|
|**Status**|**Descrição**|
|**204**|Webhook para notificações foi cancelado com sucesso.|
|**403/404/503**|Erros padrão (ver 2.1).|

## 3. Callback de Notificação (Recebimento do Webhook)

Este é o evento de notificação enviado pela instituição para a `webhookUrl` configurada.

|   |   |   |   |
|---|---|---|---|
|**Recurso**|**Método**|**Escopo**|**Descrição**|
|`{$webhookUrl}/cobr`|`POST`|N/A|Recebe a notificação de atualização de status da cobrança recorrente.|

#### Resposta Esperada do Recebedor (POST /cobr)

|   |   |
|---|---|
|**Status**|**Descrição**|
|**200**|Notificação recebida com sucesso.|

#### Request Body (Notificação Recebida)

A notificação é um objeto JSON complexo. Os campos principais são:

|   |   |   |   |
|---|---|---|---|
|**Campo**|**Tipo**|**Obrigatório**|**Descrição**|
|`idRec`|`string`|S|ID da Recorrência (29 caracteres).|
|`txid`|`string`|S|ID da Transação (Identificador único por CNPJ do recebedor).|
|`status`|`string`|S|Status atual da cobrança recorrente. **Enum:** `CRIADA`, `ATIVA`, `CONCLUIDA`, `EXPIRADA`, `REJEITADA`, `CANCELADA`.|
|`atualizacao`|`Array of objects`|S|Histórico de Status (do mais antigo ao mais recente).|
|`encerramento`|`object`|N|Detalhamento do cancelamento ou rejeição (se aplicável).|
|`tentativas`|`Array of objects`|N|Histórico de tentativas de cobrança.|
|`pix`|`Array of objects`|N|Detalhes dos Pix recebidos.|

#### Detalhamento de Status e Encerramento

|   |   |   |   |
|---|---|---|---|
|**Objeto**|**Sub-campo**|**Obrigatório**|**Enum/Códigos de Exemplo**|
|`atualizacao`|`status`|S|Mesmos valores de `status` principal.|
|`atualizacao`|`data`|S|Data e hora da atualização (`string <date-time>`).|
|`encerramento`|`Cancelamento.codigo`|S|**Exemplos:** `ACCT`, `BLCK`, `CCLD`, `FAIL`, `OTHR`, `SLBD` (Usuário Pagador), `SLCR` (Usuário Recebedor).|
|`encerramento`|`Rejeição.codigo`|S|**Exemplos:** `AB10` (Erro PSP Pagador), `AC05` (Conta Encerrada), `AM09` (Valor Divergente), `DENC` (CPF/CNPJ Divergente).|

#### Detalhamento de Pix Recebidos (`pix` array)

|   |   |   |   |
|---|---|---|---|
|**Campo**|**Tipo**|**Obrigatório**|**Descrição**|
|`endToEndId`|`string`|S|Identificador do Pix (32 caracteres).|
|`txid`|`string`|S|ID da Transação.|
|`valor`|`string`|S|Valor do Pix.|
|`horario`|`string <date-time>`|S|Horário de processamento.|
|`devolucoes`|`Array of objects`|N|Detalhes de devoluções, incluindo `id`, `rtrid`, `valor`, `natureza` (e.g., `ORIGINAL`, `MED_FRAUDE`, `MED_PIX_AUTOMATICO`).|

# 4. Dados Obrigatórios e Modelo JSON (Webhook Cobrança - `/webhookcobr`)

Esta seção detalha os campos obrigatórios e apresenta um modelo de notificação de callback (POST na `webhookUrl`) para as cobranças recorrentes.

### 4.1. Campos Obrigatórios de Alto Nível (Notificação de Cobrança)

Os seguintes campos são mandatórios no corpo do JSON recebido pela sua `webhookUrl` em um evento de notificação de cobrança:

|   |   |   |
|---|---|---|
|**Campo**|**Tipo**|**Finalidade**|
|`idRec`|`string`|Identificador único da recorrência (29 caracteres).|
|`txid`|`string`|ID da Transação da cobrança (26 a 35 caracteres).|
|`status`|`string`|Status atual do registro da cobrança.|
|`atualizacao`|`Array of objects`|Histórico de Status.|
|`atualizacao[].status`|`string`|Status dentro do histórico.|
|`atualizacao[].data`|`string <date-time>`|Data e hora da mudança de status.|
|`pix`|`Array of objects`|Array contendo o histórico de Pix recebidos (mesmo que vazio, o campo deve estar presente se houver a estrutura).|

### 4.2. Modelo JSON de Exemplo (Notificação de Cobrança CONCLUIDA)

Este modelo representa um webhook recebido quando a cobrança recorrente é **CONCLUIDA**, geralmente após o recebimento de um Pix associado à ela.

```
{
  "idRec": "RN1234567820241130lkwdefghijk",
  "txid": "3136957d93134f2184b369e8f1c0729d",
  "status": "CONCLUIDA",
  "atualizacao": [
    {
      "status": "CRIADA",
      "data": "2024-05-20T14:47:29.470Z"
    },
    {
      "status": "ATIVA",
      "data": "2024-05-21T09:00:00.000Z"
    },
    {
      "status": "CONCLUIDA",
      "data": "2024-05-21T09:05:33.123Z"
    }
  ],
  "tentativas": [
    {
      "dataLiquidacao": "2024-05-21",
      "tipo": "AGND",
      "status": "PAGA",
      "endToEndid": "E1234567820240521A1B2C3D4E5F6G7H",
      "valor": "50.00"
    }
  ],
  "pix": [
    {
      "endToEndId": "E1234567820240521A1B2C3D4E5F6G7H",
      "txid": "3136957d93134f2184b369e8f1c0729d",
      "valor": "50.00",
      "horario": "2024-05-21T09:05:33.123Z",
      "infoPagador": "Nome do Pagador",
      "devolucoes": []
    }
  ]
}
```

### 4.3. Modelo JSON de Exemplo (Notificação de Cobrança CANCELADA)

Este modelo representa um webhook recebido quando a cobrança é **CANCELADA** pelo usuário recebedor.

```
{
  "idRec": "RN1234567820241130lkwdefghijk",
  "txid": "3136957d93134f2184b369e8f1c0729d",
  "status": "CANCELADA",
  "atualizacao": [
    {
      "status": "CRIADA",
      "data": "2024-06-10T11:00:00.000Z"
    },
    {
      "status": "CANCELADA",
      "data": "2024-06-10T15:30:45.000Z"
    }
  ],
  "encerramento": {
    "Cancelamento": {
      "solicitante": "USUARIO RECEBEDOR",
      "codigo": "SLCR",
      "descricao": "Cancelamento solicitado pelo usuário recebedor."
    }
  },
  "tentativas": [],
  "pix": []
}
```