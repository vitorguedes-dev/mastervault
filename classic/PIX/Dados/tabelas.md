# Pix Automático — Documentação do Esquema (Supabase self-hosted)

> **Tabelas**: `client_pix`, `rec`, `cob`, `ledger_pix`  
> **Objetivo**: explicar a função de cada tabela, campos-chave, checks/FKs/índices, casos de uso comuns (passo a passo) e **RLS** (Row Level Security) para Supabase self-hosted.

---

## 1) Visão geral do modelo (função de cada tabela)

### 1.1 `client_pix` — Cadastro do cliente/integrador

- **Função**: representa o “dono” do vínculo/contrato no seu sistema (CRM/ERP). _Não é necessariamente o pagador do Pix._
    
- **Uso**: relaciona-se a **recorrências** (`rec.client_id_rec`) e **cobranças** (`cob.client_id_cob`).
    
- **Campos principais**:  
    `id (uuid PK)`, `nome`, `cpf (opcional)`, `nome_pagador/cpf_pagador (opcionais)`, `endereco`, `created_at/updated_at`.
    

### 1.2 `rec` — Recorrências / Autorizações (Pix Automático)

- **Função**: contrato de recorrência autorizado pelo pagador. Uma **recorrência** gera **N cobranças recorrentes** (CobR) ao longo do tempo.
    
- **Uso**: guarda periodicidade, política de retentativa, datas de vigência e **na Jornada 3** o `ativacao_txid` da primeira cobrança **imediata**.
    
- **Campos principais**:  
    `idrec (PK)`, `client_id_rec (FK->client_pix)`, `tipo_jornada`, `periodicidade`, `politica_retentativa`, `data_primeiro_pagamento`, `data_fim_vigencia`, `valor_rec XOR valor_minimo_receb`, `status_rec`, `loc (metadados)`, `atualizacao (JSONB)`, `recebedor_cnpj`, `recebedor_convenio` e **J3** `ativacao_txid (FK->cob.txid)`.
    

### 1.3 `cob` — Cobranças (unificada: imediata **e** recorrente por ciclo)

- **Função**: representa **uma cobrança** identificada por `txid` (único por recebedor). Pode ser:
    
    - `COB_IMEDIATA`: QR dinâmico (`loc_imed`, `expiracao_imed`, `chave_pix_imed`);
        
    - `COBR`: parcela da recorrência (`id_rec`, `data_vencimento`).
        
- **Uso**: guarda **estado** da cobrança e metadados do tipo. Eventos (liquidações/devoluções/tentativas) **não** moram aqui; vão para `ledger_pix`.
    
- **Campos principais**:  
    `txid (PK)`, `tipo (COB_IMEDIATA|COBR)`, `id_rec (FK opcional/obrigatório conforme tipo)`, `cnpj_recebedor`, `client_id_cob (FK->client_pix)`, `status_cob`, `data_criacao`, `valor_original`, campos específicos de cada tipo e `cancelamento/rejeicao_cliente_pix (JSONB)`.
    
- **Regra crítica**: `UNIQUE (cnpj_recebedor, txid)` garante a **unicidade real do Pix**.
    

### 1.4 `ledger_pix` — Ledger de eventos (imutável)

- **Função**: “livro-razão” com **fatos** do Pix: `LIQUIDACAO` (tem `e2eid` único), `DEVOLUCAO` (ligada ao `e2eid`), `TENTATIVA`, `STATUS_CHANGE`, `CANCELAMENTO`.
    
- **Uso**: permite 0..N liquidações por `txid` (retentativas RIFL/NTAG), 0..N devoluções por `e2eid`, auditoria, conciliação e BI.
    
- **Campos principais**:  
    `txid (FK->cob)`, `event_type`, `horario_evento`, **LIQUIDACAO**: `e2eid (unique)`, `valor`, `tentativa_tipo`, `tentativa_no`, `info_pagador`; **DEVOLUCAO**: `parent_e2eid + devolucao_id (unique pair)`, `valor`, `status`, `motivo`, `natureza`, horários; **TENTATIVA**: status e metadados.
    

---

## 2) Detalhamento por tabela (campos, checks, FKs, índices)

### 2.1 `client_pix`

- **PK**: `id uuid default gen_random_uuid()`
    
- **Campos**: `nome text not null`; `cpf text` _(regex 11 dígitos)_; `nome_pagador/cpf_pagador` _(opcionais)_; `endereco text`; `created_at/updated_at timestamptz` (com trigger `set_current_timestamp_updated_at`).
    
- **Índices**: crie `UNIQUE(external_id)` se integrar com um ID externo (não incluso por padrão).
    
- **LGPD**: persista **apenas o necessário**; avalie mascarar/criptografar `cpf*`.
    

### 2.2 `rec`

- **PK**: `idrec text` (natural).
    
- **FKs**: `client_id_rec -> client_pix(id)`; **J3** `ativacao_txid -> cob(txid)`.
    
- **Checks**:
    
    - `tipo_jornada in ('JORNADA_1','JORNADA_2','JORNADA_3','JORNADA_4','AGUARDANDO_DEFINICAO')`
        
    - `politica_retentativa in ('NAO_PERMITE','PERMITE_3R_7D')`
        
    - `periodicidade in ('SEMANAL','MENSAL','TRIMESTRAL','SEMESTRAL','ANUAL')`
        
    - `valor_rec XOR valor_minimo_receb`
        
    - **J3**: se `tipo_jornada = 'JORNADA_3'` ⇒ `ativacao_txid NOT NULL`
        
- **Índices**: `(client_id_rec)`, `(recebedor_cnpj, status_rec)`, `(periodicidade, data_primeiro_pagamento)`.
    

### 2.3 `cob`

- **PK**: `txid text` (26..35).
    
- **Unicidade**: `UNIQUE (cnpj_recebedor, txid)`
    
- **FKs**: `id_rec -> rec(idrec)` (obrigatório em `COBR`, opcional em `COB_IMEDIATA`), `client_id_cob -> client_pix(id)`.
    
- **Checks**:
    
    - `tipo in ('COB_IMEDIATA','COBR')`
        
    - **Por tipo**:  
        _`COBR`_: `id_rec` **e** `data_vencimento` **não nulos**; `loc_imed/expiracao_imed/chave_pix_imed` **nulos**.  
        _`COB_IMEDIATA`_: `loc_imed/expiracao_imed/chave_pix_imed` **não nulos**.
        
- **Índices**: `(id_rec, data_vencimento)`, `(status_cob, data_criacao)`.
    

### 2.4 `ledger_pix`

- **PK**: `id bigserial` (sintético).
    
- **FKs**: `txid -> cob(txid)`.
    
- **Unicidades condicionais**:
    
    - `UNIQUE(e2eid)` **when `event_type='LIQUIDACAO'`**
        
    - `UNIQUE(parent_e2eid, devolucao_id)` **when `event_type='DEVOLUCAO'`**
        
- **Índices**: `(txid)`, `(event_type)`, `(horario_evento)`.
    
- **Boas práticas**: particionamento mensal por `horario_evento` se o volume crescer.
    

---

## 3) Casos de uso em etapas comuns (E2E)

### 3.1 Jornada 3 — autorização + 1ª cobrança imediata

1. **Criar Cob imediata** (`cob.tipo='COB_IMEDIATA'`), gerar QR dinâmico (`loc_imed`, `expiracao_imed`, `chave_pix_imed`).
    
2. **Receber liquidação** no webhook → inserir em `ledger_pix` **LIQUIDACAO** com `e2eid`, `valor`, `horario_evento`, `tentativa_tipo` (se houver).
    
3. **Criar Recorrência** (`rec`) com `tipo_jornada='JORNADA_3'` e **`ativacao_txid` preenchido** apontando para o `txid` da 1ª Cob.
    
4. **Criar CobR futuras** (`cob.tipo='COBR'`, `id_rec`, `data_vencimento`, `valor_original`).
    
5. Em cada vencimento, registrar novas **LIQUIDACOES** / **DEVOLUCOES** no `ledger_pix`.
    

### 3.2 Jornada 2 — autorização no app do pagador (sem Cob imediata)

1. **Criar Recorrência** (`rec`) com `tipo_jornada='JORNADA_2'` (sem `ativacao_txid`).
    
2. **Criar CobR por ciclo** (`cob.tipo='COBR'`).
    
3. **Receber liquidações** e **devoluções** → `ledger_pix`.
    

### 3.3 Cancelamento de CobR antes de D-1

1. Atualizar `cob.status_cob` e registrar um **evento** `CANCELAMENTO` no `ledger_pix` com `motivo`/`horario_evento`.
    
2. (Opcional) guardar `cancelamento JSONB` na `cob` para exibir justificativa.
    

### 3.4 Devolução (total/parcial) de Pix

1. Registrar o **pedido** (evento `DEVOLUCAO` com `parent_e2eid`, `devolucao_id`, `valor`, `horario_solic`).
    
2. Atualizar com o **status/horario_liq** quando o PSP confirmar.
    
3. Relatórios calculam **líquido** somando `LIQUIDACAO` – `DEVOLUCAO`.
    

### 3.5 Consultas recorrentes

- **Cobranças de um cliente**:  
    `select * from cob where client_id_cob = :id order by data_criacao desc;`
    
- **CobR de uma recorrência**:  
    `select * from cob where id_rec = :idRec order by data_vencimento;`
    
- **Liquidações por cobrança**:  
    `select * from ledger_pix where txid = :txid and event_type='LIQUIDACAO';`
    
- **Devoluções de um e2eid**:  
    `select * from ledger_pix where parent_e2eid = :e2eid and event_type='DEVOLUCAO';`
    

---

## 4) RLS (Row Level Security) — Supabase self-hosted

> **Padrão**: RLS **habilitado** nas 4 tabelas; o **service_role** do backend ignora RLS e pode operar livremente (webhooks, jobs).  
> **Público/clients**: crie políticas específicas. Abaixo estão modelos práticos.

### 4.1 Premissas para RLS

- O **UUID do usuário autenticado** (`auth.uid()`) será **igual** ao `client_pix.id` quando você quiser que um usuário enxergue **apenas seus próprios registros**.  
    _(Se não for o caso, adicione um campo `owner_user_id` em `client_pix` e ajuste as políticas.)_
    

### 4.2 Políticas sugeridas

**Leitura de clientes (somente o próprio)**

`create policy "client_pix_select_own" on public.client_pix for select to authenticated using ( id = auth.uid() );`

**Leitura de recorrências do cliente**

`create policy "rec_select_by_client" on public.rec for select to authenticated using ( client_id_rec = auth.uid() );`

**Leitura de cobranças do cliente**

`create policy "cob_select_by_client" on public.cob for select to authenticated using ( client_id_cob = auth.uid() );`

**Leitura do ledger por vínculo à cobrança do cliente**

`create policy "ledger_select_by_client_cob" on public.ledger_pix for select to authenticated using ( exists (   select 1 from public.cob c   where c.txid = ledger_pix.txid     and c.client_id_cob = auth.uid() ) );`

> **Escrita**: normalmente restrita ao backend (**service_role**). Caso um client precise criar leituras/rascunhos, crie políticas `insert/update` similares, **limitadas ao próprio `auth.uid()`** e com **CHECKs** (ex.: impedir mudança de FKs para outro cliente).

### 4.3 Realtime (opcional)

Se você usa **Realtime**, adicione as tabelas à publicação `supabase_realtime` (o script de schema já faz isso se a publicação existir).

---

## 5) Boas práticas adicionais

- **Índices** coerentes com as queries e particionamento mensal do `ledger_pix` se o volume aumentar.
    
- **Valores monetários** sempre em `NUMERIC(15,2)` (ou inteiros em centavos).
    
- **Timestamps** críticos em `TIMESTAMPTZ`.
    
- **Auditoria**: mantenha `atualizacao (JSONB)` ou uma tabela de histórico — mas **eventos de Pix** sempre no `ledger_pix`.
    
- **LGPD**: minimize PII (principalmente do **pagador**); retenção e acesso apenas ao necessário.
    
- **Views/materialized views**: para relatórios (ex.: liquidadas no período, MRR).
    

---

## 6) Referência rápida (chaves e regras)

- `cob`: `PRIMARY KEY(txid)`, `UNIQUE(cnpj_recebedor, txid)`, `CHECK` por tipo, FKs `id_rec` e `client_id_cob`.
    
- `rec`: `PRIMARY KEY(idrec)`, FK `client_id_rec`, **J3** FK `ativacao_txid -> cob(txid)`, XOR de valores, periodicidade/política válidas.
    
- `ledger_pix`: `UNIQUE(e2eid)` (quando `LIQUIDACAO`), `UNIQUE(parent_e2eid, devolucao_id)` (quando `DEVOLUCAO`).
    
- `client_pix`: PK `id`, RLS por `id = auth.uid()` quando desejar multitenancy por usuário.

---
## Credenciais

URL: https://supa-pix.masterclassic.app/
User: zKYf0U0NsVDS6ETW
Passwd: SbzSYbAC3WoFsunqAW66FysjyECa18ul