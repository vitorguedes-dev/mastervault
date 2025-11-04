# Documentação da API: Pix Automático - Webhook Recorrência (Autorização)

**Objetivo:** Gerenciamento e notificação de autorizações de recorrência (vínculos) via Webhook.

**Versão da Especificação:** 07/2025 (Spec Bacen 2.8.2)

## 1. Configurações de Conexão

|   |   |   |   |
|---|---|---|---|
|**Ambiente**|**URL Base**|**Header Authorization**|**Query Param gw-dev-app-key**|
|**Homologação**|`https://api-pix.hm.bb.com.br/pix/v2`|`Bearer seu Token`|`suaAppKeyDeHomologacao`|
|**Produção**|`https://api-pix.bb.com.br/pix/v2`|`Bearer seu Token`|`suaAppKeyDeProducao`|
|**Header Fixo**|`Content-Type: application/json`|-|-|

## 2. Endpoints da API

### 2.1. Configurar WebhookRec (Ativação)

|               |            |                    |                                                                                                                            |
| ------------- | ---------- | ------------------ | -------------------------------------------------------------------------------------------------------------------------- |
| **Recurso**   | **Método** | **Escopo**         | **Descrição**                                                                                                              |
| `/webhookrec` | `PUT`      | `webhookrec.write` | Envia a URL para onde as notificações de autorizações de recorrência (associadas ao CNPJ do recebedor) devem ser enviadas. |

#### Request Body (PUT /webhookrec)

|   |   |   |   |   |
|---|---|---|---|---|
|**Campo**|**Tipo**|**Obrigatório**|**Limite**|**Exemplo**|
|`webhookUrl`|`string <uri>`|S|$\le 140$ caracteres|`https://usuario.recebedor.com/api/webhookrec/`|

#### Respostas (PUT /webhookrec)

|   |   |
|---|---|
|**Status**|**Descrição**|
|**200**|Configuração e ativação do serviço bem-sucedida.|
|**400/403/404/503**|Erros padrão da API.|

### 2.2. Consultar Webhook Cadastrado

|               |            |                   |                                                 |
| ------------- | ---------- | ----------------- | ----------------------------------------------- |
| **Recurso**   | **Método** | **Escopo**        | **Descrição**                                   |
| `/webhookrec` | `GET`      | `webhookrec.read` | Retorna os dados de configuração do webhookrec. |

#### Respostas (GET /webhookrec)

|   |   |   |   |
|---|---|---|---|
|**Status**|**Campo (JSON Body)**|**Tipo**|**Descrição**|
|**200**|`webhookUrl`|`string <uri>`|URL configurada (Obrigatório).|
|**200**|`criacao`|`string <date-time>`|Data e hora em que o webhook foi cadastrado (Não Obrigatório).|
|**404**|-|-|Webhook não configurado/ativado.|

### 2.3. Cancelar Webhook

|   |   |   |   |
|---|---|---|---|
|**Recurso**|**Método**|**Escopo**|**Descrição**|
|`/webhookrec`|`DELETE`|`webhookrec.write`|Cancela o cadastramento do webhook de recorrências.|

#### Respostas (DELETE /webhookrec)

|   |   |
|---|---|
|**Status**|**Descrição**|
|**204**|Webhook para notificações foi cancelado com sucesso.|
|**403/404/503**|Erros padrão da API.|

## 3. Callback de Notificação (Recebimento do Webhook)

Este é o evento enviado para a `webhookUrl` do recebedor, notificando sobre alterações na recorrência.

|   |   |   |
|---|---|---|
|**Recurso**|**Método**|**Resposta Esperada**|
|`{$webhookUrl}/rec`|`POST`|Status **200** (Notificação recebida com sucesso).|

### Request Body (Notificação Recebida)

|   |   |   |   |
|---|---|---|---|
|**Campo**|**Tipo**|**Obrigatório**|**Observações**|
|`idRec`|`string`|**S**|ID da Recorrência (29 caracteres). Recorrências iniciadas com "C" (Open Finance) não são amparadas.|
|`status`|`string`|**S**|Status atual da recorrência. **Enum:** `CRIADA`, `APROVADA`, `REJEITADA`, `EXPIRADA`, `CANCELADA`.|
|`atualizacao`|`Array of objects`|**S**|Histórico das mudanças de status (do mais antigo para o mais recente).|
|`encerramento`|`object`|N|Detalhamento da **Rejeição** ou **Cancelamento** da recorrência.|
|`ativacao`|`object`|N|Dados da confirmação da ativação da recorrência (Jornada).|

### Detalhamento da Ativação (`ativacao`)

|   |   |   |   |
|---|---|---|---|
|**Sub-campo**|**Tipo**|**Obrigatório**|**Enum/Observações**|
|`tipoJornada`|`string`|S|Enum: `JORNADA_1` (Notificação), `JORNADA_2` (QR Code Recorrência), `JORNADA_3` (QR Code Composto c/ Cobrança Imediata), `JORNADA_4` (QR Code Composto c/ Cobrança Vencimento/Estática), `AGUARDANDO_DEFINICAO`.|
|`dadosJornada`|`object`|N (S se `tipoJornada` for `JORNADA_3`)|Contém o campo `txid` (Id da Transação da Cobrança Imediata) se a jornada for `JORNADA_3`.|

### Detalhamento do Encerramento (`encerramento`)

|   |   |   |   |
|---|---|---|---|
|**Sub-objeto**|**Campo**|**Obrigatório**|**Descrição / Códigos de Exemplo**|
|**Rejeição**|`codigo`|S|**AP13** (Rejeitada pelo pagador por não reconhecimento do recebedor); **AP14** (Rejeitada pelo pagador por falta de interesse).|
|**Rejeição**|`descricao`|S|Descrição da causa da rejeição ($\le 105$ caracteres).|
|**Cancelamento**|`solicitante`|S|Enum: `PSP PAGADOR`, `USUARIO PAGADOR`, `PSP RECEBEDOR`, `USUARIO RECEBEDOR`.|
|**Cancelamento**|`codigo`|S|**SLCR** (Cancelamento solicitado pelo recebedor); **SLDB** (Cancelamento solicitado pelo pagador); **ACCL** (Cancelamento da conta).|
|**Cancelamento**|`descricao`|S|Descrição da causa do cancelamento ($\le 105$ caracteres).|

## 4. Dados Obrigatórios e Modelo JSON

### 4.1. Campos Obrigatórios de Alto Nível (Notificação)

|   |   |   |
|---|---|---|
|**Campo**|**Tipo**|**Finalidade**|
|`idRec`|`string`|Identificador único da recorrência.|
|`status`|`string`|Status atual do ciclo de vida da recorrência.|
|`atualizacao`|`Array`|Histórico de status.|
|`atualizacao[].status`|`string`|Status dentro do histórico.|
|`atualizacao[].data`|`string <date-time>`|Data da mudança de status.|

### 4.2. Modelo JSON de Exemplo (Notificação de Recorrência APROVADA)

Este modelo representa um webhook recebido quando a recorrência é ativada via Jornada 3 (QR Code Composto e Cobrança Imediata).

```
{
  "idRec": "RN1234567820241130lkwdefghijk",
  "status": "APROVADA",
  "atualizacao": [
    {
      "status": "CRIADA",
      "data": "2024-01-02T10:00:00.000Z"
    },
    {
      "status": "APROVADA",
      "data": "2024-01-03T08:30:02.050Z"
    }
  ],
  "ativacao": {
    "tipoJornada": "JORNADA_3",
    "dadosJornada": {
      "txid": "33beb661beda44a8928fef47dbeb2dc5"
    }
  }
}
```

### 4.3. Modelo JSON de Exemplo (Notificação de Recorrência REJEITADA)

Este modelo representa um webhook recebido quando o usuário pagador rejeita o vínculo.

```
{
  "idRec": "RN1234567820241130lkwdefghijk",
  "status": "REJEITADA",
  "atualizacao": [
    {
      "status": "CRIADA",
      "data": "2024-01-02T10:00:00.000Z"
    },
    {
      "status": "REJEITADA",
      "data": "2024-01-03T08:35:10.000Z"
    }
  ],
  "encerramento": {
    "Rejeição": {
      "codigo": "AP14",
      "descricao": "Solicitação de confirmação da recorrência rejeitada pelo usuário pagador por não ter interesse no uso do Pix Automático."
    }
  }
}
```