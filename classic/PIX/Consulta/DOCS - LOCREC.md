# Documentação da API: Pix Automático - Location de Recorrência (Autorização)

**Recurso Base:** `/locrec`

**Objetivo:** Gerenciar as Locations (Localizações) dos QR Codes de Recorrência, que são usados para iniciar o processo de autorização do Pix Automático pelo pagador (Jornadas J2, J3 e J4).

**Versão da Especificação:** 07/2025 (Spec Bacen 2.8.2).

## 1. Resumo de Funcionalidades (Endpoint `/locrec`)

O endpoint `/locrec` permite ao usuário recebedor executar as seguintes operações focadas na localização da recorrência:

1. **Criar Location:** Criar a localização para o payload da recorrência (`POST /locrec`). Este passo é necessário antes de criar a recorrência final.
    
2. **Consultar Location:** Consultar os dados de uma Location existente, incluindo o ID da Location, o campo `location` (URL do QR Code) e o `idRec` associado (`GET /locrec/{id}`).
    
3. **Cancelar Location:** Cancelar uma Location que não será mais utilizada (`DELETE /locrec/{id}`).
    
4. **Consultar Locations:** Listar todas as Locations existentes do usuário recebedor (`GET /locrec`).
    

## 2. Configurações de Conexão

|   |   |   |   |
|---|---|---|---|
|**Ambiente**|**URL Base**|**Header Authorization**|**Query Param gw-dev-app-key**|
|**Homologação (mTLS)**|`https://api-pix.hm.bb.com.br/pix/v2`|`Bearer seu Token`|`suaAppKeyDeHomologacao`|
|**Produção (mTLS)**|`https://api-pix.bb.com.br/pix/v2`|`Bearer seu Token`|`suaAppKeyDeProducao`|
|**Header Fixo**|`Content-Type: application/json`|-|-|

## 3. Endpoints da API

### 3.1. Criar Location de Recorrência

|   |   |   |   |
|---|---|---|---|
|**Recurso**|**Método**|**Escopo**|**Descrição**|
|`/locrec`|`POST`|`payloadlocationrec.write`|Cria o objeto de Location que será usado para gerar o QR Code.|

#### Request Body (POST /locrec) - Campos Principais

|   |   |   |   |
|---|---|---|---|
|**Campo**|**Tipo**|**Obrigatório**|**Detalhes**|
|`idRec`|`string`|N|Identificador da Recorrência ($\le 29$ caracteres). Opcional na criação, obrigatório na consulta do QR Code.|
|`descricaoVinc`|`string`|**S**|Descrição do Vínculo ($\le 140$ caracteres). Campo de texto livre para o pagador reconhecer a finalidade.|

#### Resposta (POST /locrec)

|   |   |   |   |
|---|---|---|---|
|**Status**|**Campo (JSON Body)**|**Tipo**|**Descrição**|
|**201**|`id`|`integer <int64>`|Identificador da location (Obrigatório).|
|**201**|`location`|`string <uri>`|URL da Location (Obrigatório).|
|**201**|`criacao`|`string <date-time>`|Data e hora de criação (Obrigatório).|
|**201**|`idRec`|`string`|ID da Recorrência (Não Obrigatório, se não informado no request).|

### 3.2. Consultar Location de Recorrência

|   |   |   |   |
|---|---|---|---|
|**Recurso**|**Método**|**Escopo**|**Descrição**|
|`/locrec/{id}`|`GET`|`payloadlocationrec.read`|Retorna os dados da Location pelo seu ID.|

### 3.3. Cancelar Location de Recorrência

|   |   |   |   |
|---|---|---|---|
|**Recurso**|**Método**|**Escopo**|**Descrição**|
|`/locrec/{id}`|`DELETE`|`payloadlocationrec.write`|Cancela uma Location existente, tornando-a inutilizável.|

#### Resposta (DELETE /locrec/{id})

|   |   |
|---|---|
|**Status**|**Descrição**|
|**204**|Location cancelada com sucesso.|

### 3.4. Consultar Locations (Lista)

|   |   |   |   |
|---|---|---|---|
|**Recurso**|**Método**|**Escopo**|**Descrição**|
|`/locrec`|`GET`|`payloadlocationrec.read`|Retorna uma lista de Locations cadastradas.|

#### Query Parameters (GET /locrec)

|   |   |   |   |
|---|---|---|---|
|**Parâmetro**|**Tipo**|**Obrigatório**|**Descrição**|
|`status`|`string`|N|Filtra por status: `ATIVA` (default) ou `REMOVIDA`.|
|`idRec`|`string`|N|Filtra por Recorrência ID.|
|`paginacao.paginaAtual`|`integer`|N|Número da página.|
|`paginacao.itensPorPagina`|`integer`|N|Itens por página ($\le 2000$).|

## 4. Dados Obrigatórios e Modelo JSON (Criação de Location)

### 4.1. Campos Obrigatórios Mínimos (POST /locrec)

|   |   |   |
|---|---|---|
|**Campo**|**Tipo**|**Finalidade**|
|`descricaoVinc`|`string`|Campo de texto livre para informar o pagador sobre o motivo da recorrência.|

### 4.2. Modelo JSON de Exemplo (Criação de Location - Request)

Este é o JSON enviado ao endpoint `POST /locrec`:

```
{
  "descricaoVinc": "Pagamento mensal da assinatura premium de software X. Consulte seu e-mail para detalhes."
}
```

### 4.3. Modelo JSON de Exemplo (Criação de Location - Response 201)

Este é o JSON retornado após a criação bem-sucedida da Location:

```
{
  "id": 12069,
  "location": "[pix.example.com/qr/v2/rec/1234567890abcdef1234567890abcdef](https://pix.example.com/qr/v2/rec/1234567890abcdef1234567890abcdef)",
  "criacao": "2025-10-21T11:25:00.000Z",
  "idRec": "RN1234567820241130lkwdefghijk",
  "status": "ATIVA"
}
```