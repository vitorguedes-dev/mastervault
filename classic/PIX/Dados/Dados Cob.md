# Especificação de Dados — Cobrança Recorrente (Pix Automático /cobr)

> **Objetivo**: descrever todos os dados de uma **Cobrança Recorrente** (recurso **CobR**) para modelagem de banco de dados e conciliação.

---

## 1) Identificação e vínculos

- **txid** — _string (26–35, obrigatório)_  
    Identificador **único por recebedor** da cobrança deste ciclo. **Não reutilizar** entre ciclos.
    
- **idRec** — _string (29, obrigatório)_  
    Identificador da **recorrência** à qual a CobR pertence (relação 1:N).
    

---

## 2) Calendário da CobR

- **calendario** — _object (obrigatório)_
    
    - **dataDeVencimento** — _date (YYYY-MM-DD, obrigatório)_: data prevista para a tentativa principal.
        
    - _(Observação)_: retentativas **pós-vencimento** (NTAG) seguem a **política de retentativa** da recorrência; reagendamentos usam o endpoint específico de retentativa. _(Para detalhes exatos de limites/janelas, **consultar documentação oficial do BB**.)_
        

---

## 3) Valor

- **valor** — _object (obrigatório)_
    
    - **original** — _string monetária com 2 casas (obrigatório)_: valor a cobrar neste ciclo.
        
    - _(Regra)_: **não é permitido alterar** o valor após criar a CobR. Para corrigir: **cancelar** e **criar nova** CobR com **novo `txid`**.
        

---

## 4) Status e encerramento

- **status** — _enum (obrigatório)_: `CRIADA | ATIVA | CONCLUIDA | EXPIRADA | REJEITADA | CANCELADA`.
    
- **encerramento** — _object (opcional)_
    
    - **cancelamento** — _object_ (quando `status = CANCELADA`)
        
        - **solicitante** — `PSP_PAGADOR | USUARIO_PAGADOR | PSP_RECEBEDOR | USUARIO_RECEBEDOR`
            
        - **codigo** — motivo padronizado (_ex._ `SLCR`, `SLBD`, `ACCT`, `BLCK`, `CCLD`, `FAIL`, `OTHR`)
            
        - **descricao** — _string ≤ 105_
            
    - _(Observação)_: rejeições por regra do pagador podem constar como `REJEITADA` com códigos de domínio (ex.: **AM02**, **AM09**).
        

---

## 5) Liquidações e devoluções (eventos Pix)

- **pix** — _array (0..N)_: eventos de **liquidação** atrelados a este `txid`
    
    - **e2eid** — _string (32, obrigatório)_
        
    - **valor** — _string monetária (obrigatório)_
        
    - **horario** — _date-time RFC 3339 (obrigatório)_
        
    - **devolucoes** — _array (0..N, opcional)_
        
        - **id** — _string_
            
        - **valor** — _string monetária_
            
        - **status** — _enum_
            
        - **horario** — _date-time_
            
    - _(Nota)_: em retentativa **RIFL/NTAG**, uma nova liquidação gera **novo `e2eid`** para o **mesmo `txid`**.
        

> **Sugestão de modelagem**: normalizar eventos Pix por **`e2eid`** em tabela própria e manter `txid` como FK (facilita auditoria/devoluções).

---

## 6) Webhooks e auditoria

- **WebhookCobr** — callbacks `POST {webhookUrl}/cobr` com `txid`, `status` e (quando houver) `encerramento`.
    
- **Linha do tempo** — se o payload do PSP não trouxer histórico, mantenha **trilha interna** (status + timestamps).
    

---

## 7) Campos adicionais (quando fornecidos)

- **loc** — _object (opcional)_: metadados da _location_ associada (se aplicável à jornada).
    
- **naturezaDevolucao** — _enum (opcional)_: natureza informada nas devoluções (ex.: `MED_PIX_AUTOMATICO`).
    
- **observacoes** — campos auxiliares específicos do PSP (ex.: convênio, headers). _(Quando não padronizados, documente internamente.)_
    

---

## 8) Regras de negócio

- **Cancelamento**: até **D-1** (janela operacional ~ **22:00 do dia anterior**). Endpoint: `PATCH /cobr/{txid}` com `{ "status": "CANCELADA" }`.
    
- **Retentativa**: `POST /cobr/{txid}/retentativa/{data}` conforme a **política da recorrência**.
    
- **Idempotência**: `txid` **não** pode ser reutilizado.
    

---

## 9) Índices sugeridos

- UNIQUE(`txid`)
    
- INDEX(`idRec`, `status`, `calendario.dataDeVencimento`)
    
- INDEX(`status`, `updatedAt`)
    

---

# `cobranca_imediata_campos.md`

# Especificação de Dados — Cobrança Imediata (Pix /cob)

> **Objetivo**: descrever todos os dados de uma **Cobrança Imediata** (recurso **Cob**) do Pix “clássico”, inclusive para a **Jornada 3** (1ª cobrança + autorização).

---

## 1) Identificação e QR dinâmico

- **txid** — _string (26–35, obrigatório)_: identificador **único por recebedor**.
    
- **loc** — _object (obrigatório para QR dinâmico)_
    
    - **id** — _int64 (obrigatório)_
        
    - **location** — _URI ≤ 77 (obrigatório)_
        
    - **criacao** — _date-time (obrigatório)_
        

---

## 2) Calendário

- **calendario** — _object (obrigatório)_
    
    - **expiracao** — _int (segundos, obrigatório)_: tempo de validade do QR dinâmico.
        
    - _(Observação)_: podem existir campos adicionais como `criacao`/`apresentacao` conforme implementação. **Consultar documentação oficial do BB** para o conjunto exato.
        

---

## 3) Devedor (opcional)

- **devedor** — _object (opcional)_
    
    - **cpf** — _string (11)_ **ou** **cnpj** — _string (14)_
        
    - **nome** — _string ≤ 140_
        

---

## 4) Valor e chave Pix

- **valor** — _object (obrigatório)_
    
    - **original** — _string monetária com 2 casas (obrigatório)_
        
- **chave** — _string (obrigatório)_: chave Pix do recebedor (EVP/telefone/e-mail/aleatória).
    

---

## 5) Mensagem ao pagador

- **solicitacaoPagador** — _string ≤ 140 (opcional)_.
    

---

## 6) Status e revisão

- **status** — _enum_: `ATIVA | CONCLUIDA | REMOVIDA_PELO_USUARIO_RECEBEDOR | REMOVIDA_PELO_PSP`
    
- _(Observação)_: alterações pontuais podem ser feitas via `PATCH /cob/{txid}` (conforme base BACEN). **Consultar documentação oficial do BB** para quais campos são mutáveis.
    

---

## 7) Liquidações e devoluções (eventos Pix)

- **pix** — _array (0..N)_
    
    - **e2eid** — _string (32, obrigatório)_
        
    - **valor** — _string monetária (obrigatório)_
        
    - **horario** — _date-time RFC 3339 (obrigatório)_
        
    - **devolucoes** — _array (0..N, opcional)_ com `id`, `valor`, `status`, `horario`
        

> **Jornada 3**: o `txid` desta **Cob** é referenciado em `rec.ativacao.dadosJornada.txid` para compor o **QR composto** (1ª cobrança + autorização).

---

## 8) Webhook e conciliação

- **Webhook Pix (clássico)** — cadastrado por **chave** (`PUT /webhook/{chave}`) para receber eventos de Pix recebidos ligados ao `txid`.
    
- **Consulta por período** — `GET /pix?inicio&fim` (quando necessário) para varreduras/conciliar extrato.
    

---

## 9) Regras de negócio

- **Idempotência**: preferir **`PUT /cob/{txid}`** para criar com `txid` próprio; `POST /cob` pode não ser idempotente.
    
- **Expiração**: `calendario.expiracao` governa a validade do QR.
    
- **Devolução**: `PUT /pix/{e2eid}/devolucao/{id}` e `GET` correspondente para acompanhar.
    

---

## 10) Índices sugeridos

- UNIQUE(`txid`)
    
- INDEX(`status`, `updatedAt`)
    
- INDEX por período de criação/expiração