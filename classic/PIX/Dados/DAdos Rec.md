
# Especificação de Dados — Recorrência (Pix Automático /rec)

> **Objetivo**: descrever todos os dados de uma *recorrência* (entidade **Rec**) para modelagem de banco de dados, com obrigatoriedade, tipos, regras e observações.

---

## 1) Identificação
- **idRec** — *string (29 chars, obrigatório)*  
  - Identificador da recorrência. Prefixos:
    - **RR…**: criada via API Pix com retentativa.
    - **RN…**: criada via API Pix sem retentativa.
  - IDs iniciados por **C…** (Open Finance) **não** são manipuláveis por esta API.

---

## 2) Vínculo (descrição do objeto da recorrência)
- **vinculo** — *object (obrigatório)*
  - **objeto** — *string ≤ 35 (opcional)*: texto para o usuário reconhecer o contrato/serviço.
  - **contrato** — *string ≤ 35 (obrigatório)*: código/número que representa o contrato/pedido e o vínculo entre recebedor e devedor.
  - **devedor** — *object (obrigatório)*:
    - **cpf** — *string (11 dígitos)* **ou** **cnpj** — *string (14 dígitos)*: um dos dois é obrigatório.
    - **nome** — *string ≤ 140 (obrigatório)*.

---

## 3) Calendário da recorrência
- **calendario** — *object (obrigatório)*
  - **dataInicial** — *date (YYYY-MM-DD, obrigatório)*: data estimada do primeiro pagamento.
  - **dataFinal** — *date (YYYY-MM-DD, opcional)*: fim de vigência; compatível com a periodicidade; deixar ausente para prazo indeterminado.
  - **periodicidade** — *enum (obrigatório)*: `SEMANAL | MENSAL | TRIMESTRAL | SEMESTRAL | ANUAL`.

---

## 4) Valor (parâmetros econômicos)
- **valor** — *object (opcional)*
  - **valorRec** — *string monetária (2 casas, opcional)*: informar quando os pagamentos forem de **valor fixo** durante a vigência.
  - **valorMinimoRecebedor** — *string monetária (opcional)*: piso definido pelo recebedor para autorizações **não fixas**. Se o pagador definir um teto, ele **não** pode ser inferior a este piso. **Mutuamente exclusivo** com `valorRec`.

---

## 5) Recebedor
- **recebedor** — *object (obrigatório)*
  - **ispbParticipante** — *string (8 dígitos, obrigatório)*: ISPB do recebedor.
  - **cnpj** — *string (14 dígitos, obrigatório)*.
  - **nome** — *string ≤ 140 (obrigatório)*.
  - **convenio** — *string ≤ 60 (condicional/opcional)*: obrigatório quando o CNPJ possuir mais de um convênio junto ao PSP recebedor.

---

## 6) Pagador
- **pagador** — *object (opcional)*
  - **ispbParticipante** — *string (8 dígitos, opcional)*.
  - **codMun** — *string (7 dígitos IBGE, opcional)*.
  - **cpf** — *string (11)* **ou** **cnpj** — *string (14)* (quando aplicável).

---

## 7) Status e histórico
- **status** — *enum (obrigatório)*: `CRIADA | APROVADA | REJEITADA | EXPIRADA | CANCELADA`.
- **atualizacao** — *array (obrigatório)*: histórico de mudanças de status (do mais antigo ao mais recente), cada item com:
  - **status** — *enum* (um dos acima).
  - **data** — *date-time (RFC 3339)*.

---

## 8) Política de retentativa pós-vencimento
- **politicaRetentativa** — *enum (obrigatório)*: `NAO_PERMITE | PERMITE_3R_7D`.

---

## 9) Location (para construção/exibição do QR de autorização)
- **loc** — *object (obrigatório na criação/retorno)*
  - **id** — *int64 (obrigatório)*.
  - **location** — *URI ≤ 77 (obrigatório)*.
  - **criacao** — *date-time (obrigatório)*.

---

## 10) Ativação (confirmação da autorização)
- **ativacao** — *object (retornado/conforme jornada)*
  - **tipoJornada** — *enum*: `JORNADA_1 | JORNADA_2 | JORNADA_3 | JORNADA_4 | AGUARDANDO_DEFINICAO`  
    - Na criação, fica como **AGUARDANDO_DEFINICAO** até a autorização.
  - **dadosJornada** — *object (condicional)*:
    - **txid** — *string (26–35, obrigatório na **Jornada 3**)*: `txid` da **1ª cobrança imediata** usada para compor o QR de J3. `txid` é **único por CPF/CNPJ do recebedor**.

---

## 11) Encerramento (oneOf: rejeição **ou** cancelamento)
- **encerramento.rejeicao** — *object*
  - **codigo** — *enum* (p.ex., `AP13`, `AP14`).
  - **descricao** — *string ≤ 105*.
- **encerramento.cancelamento** — *object*
  - **solicitante** — *enum*: `PSP_PAGADOR | USUARIO_PAGADOR | PSP_RECEBEDOR | USUARIO_RECEBEDOR`.
  - **codigo** — *enum* (p.ex., `ACCL`, `CPCL`, `DCSD`, `FRUD`, `PCFD`, `SLCR`, `SLDB`).
  - **descricao** — *string ≤ 105*.
> Observação: `rejeicao` e `cancelamento` são **excludentes** (estrutura *oneOf*).

---

## 12) Dados para exibição (retorno em GET /rec/{idRec} quando aplicável)
- **dadosQR** — *object (somente em consultas; não persiste na criação)*
  - **jornada** — *enum*: `JORNADA_2 | JORNADA_3 | JORNADA_4`.
  - **pixCopiaECola** — *string ≤ 512*: conteúdo a ser convertido em QR conforme a jornada (na **J3**, é o **QR composto** da 1ª cobrança + recorrência).

---

## 13) Observações importantes para modelagem
- **Escopo do idRec**: IDs que iniciam com **R…** pertencem à API Pix; IDs com **C…** são do Open Finance e não são manipuláveis aqui.
- **Retentativa**: a escolha da política (`politicaRetentativa`) é por recorrência e influencia retentativas pós-vencimento (ex.: até 3 em 7 dias).
- **Valor**: `valorRec` e `valorMinimoRecebedor` são **mutuamente exclusivos**.
- **Histórico**: armazene **todo** o array `atualizacao[]` (status + data RFC 3339) para auditoria e reconciliação.
