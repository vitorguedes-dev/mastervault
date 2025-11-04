# Documentação da API: Pix Automático - Recorrências (Autorizações)

**Recurso Base:** `/rec`

**Objetivo:** Gerenciamento do ciclo de vida das autorizações de débito recorrente (Recorrências) entre o usuário recebedor e o usuário pagador. Esta é a API principal para criar, consultar e cancelar a recorrência.

**Versão da Especificação:** 07/2025 (Spec Bacen 2.8.2)

## 1. Resumo de Funcionalidades (Endpoint `/rec`)

O endpoint `/rec` permite ao usuário recebedor executar as seguintes operações essenciais de gerenciamento de autorização:

1. **Criar** uma nova solicitação de autorização de recorrência (`POST /rec`).
    
2. **Consultar** os detalhes e o status de uma recorrência específica (`GET /rec/{idRec}`).
    
3. **Cancelar** uma recorrência que não é mais desejada ou necessária (`DELETE /rec/{idRec}`).
    
4. **Consultar** o histórico de pagamentos Pix efetivados dentro de uma recorrência (`GET /rec/{idRec}/pix`).
    

## 2. Configurações de Conexão

|   |   |   |   |
|---|---|---|---|
|**Ambiente**|**URL Base**|**Header Authorization**|**Query Param gw-dev-app-key**|
|**Homologação (mTLS)**|`https://api-pix.hm.bb.com.br/pix/v2`|`Bearer seu Token`|`suaAppKeyDeHomologacao`|
|**Produção (mTLS)**|`https://api-pix.bb.com.br/pix/v2`|`Bearer seu Token`|`suaAppKeyDeProducao`|
|**Header Fixo**|`Content-Type: application/json`|-|-|

## 3. Endpoints da API

### 3.1. Criar Recorrência

|   |   |   |   |
|---|---|---|---|
|**Recurso**|**Método**|**Escopo**|**Descrição**|
|`/rec`|`POST`|`rec.write`|Inicia o processo de autorização, criando o registro da recorrência.|

#### Request Body (POST /rec) - Campos Principais

|   |   |   |   |
|---|---|---|---|
|**Campo**|**Tipo**|**Obrigatório**|**Detalhes**|
|`idRec`|`string`|**S**|Identificador único da recorrência ($\le 29$ caracteres).|
|`txid`|`string`|**S**|Identificador da transação ($\le 35$ caracteres).|
|`periodicidade`|`string`|**S**|Enum: `MENSAL`, `DIARIA`, `SEMANAL`, `QUINZENAL`, `ANUAL`.|
|`calendario`|`object`|**S**|Deve conter a `dataDeCriacao` (`string <date-time>`).|
|`devedor`|`object`|**S**|Deve conter `cpf` ou `cnpj` do pagador.|
|`valor`|`object`|**S**|Deve conter o `original` (`string`) da recorrência.|
|`configuracao`|`object`|**S**|Deve conter `expiracao` (`string <date-time>`) e `valorVaria` (`boolean`).|

#### Resposta (POST /rec)

|   |   |
|---|---|
|**Status**|**Descrição**|
|**201**|Recorrência criada com sucesso. Status inicial: `CRIADA`.|

### 3.2. Consultar Recorrência

|   |   |   |   |
|---|---|---|---|
|**Recurso**|**Método**|**Escopo**|**Descrição**|
|`/rec/{idRec}`|`GET`|`rec.read`|Retorna o JSON completo da recorrência.|

### 3.3. Cancelar Recorrência

|   |   |   |   |
|---|---|---|---|
|**Recurso**|**Método**|**Escopo**|**Descrição**|
|`/rec/{idRec}`|`DELETE`|`rec.write`|Cancela uma recorrência existente.|

#### Resposta (DELETE /rec/{idRec})

|   |   |
|---|---|
|**Status**|**Descrição**|
|**204**|Recorrência cancelada com sucesso.|

### 3.4. Consultar Pix de uma Recorrência

|   |   |   |   |
|---|---|---|---|
|**Recurso**|**Método**|**Escopo**|**Descrição**|
|`/rec/{idRec}/pix`|`GET`|`rec.read`|Retorna a lista de todos os pagamentos Pix efetivados dentro da recorrência.|

## 4. Dados Obrigatórios e Modelo JSON (Criação de Recorrência)

### 4.1. Campos Obrigatórios Mínimos (POST /rec)

|   |   |   |   |
|---|---|---|---|
|**Campo**|**Subcampo**|**Tipo**|**Finalidade**|
|`idRec`|-|`string`|Identificador único.|
|`txid`|-|`string`|Identificador da transação.|
|`periodicidade`|-|`string`|Frequência (e.g., `MENSAL`).|
|`calendario`|`dataDeCriacao`|`string <date-time>`|Data de criação.|
|`devedor`|`cpf` ou `cnpj`|`string`|Documento do pagador.|
|`valor`|`original`|`string`|Valor base (decimal, e.g., "100.00").|
|`configuracao`|`expiracao`|`string <date-time>`|Data limite.|
|`configuracao`|`valorVaria`|`boolean`|Indica se o valor pode ser alterado.|

### 4.2. Modelo JSON de Exemplo (Criação Mínima)

Este modelo demonstra a estrutura mínima requerida para iniciar uma recorrência no endpoint `POST /rec`.

```
{
  "idRec": "RN1234567820251130lkwdefghijk",
  "txid": "78762d2d93134f2184b369e8f1c0729d",
  "calendario": {
    "dataDeCriacao": "2025-10-21T10:10:00.000Z"
  },
  "devedor": {
    "cpf": "12345678909"
  },
  "valor": {
    "original": "100.00"
  },
  "periodicidade": "MENSAL",
  "configuracao": {
    "expiracao": "2026-10-21T23:59:59.000Z",
    "valorVaria": false
  },
  "infoAdicionais": []
}
```