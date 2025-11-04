# Documentação Completa da Plataforma

## 📋 Índice

1. [Visão Geral](#visão-geral)
2. [Requisitos e Setup Inicial](#requisitos-e-setup-inicial)
3. [Autenticação e Usuários](#autenticação-e-usuários)
4. [Módulos Principais](#módulos-principais)
5. [Integrações](#integrações)
6. [APIs e Edge Functions](#apis-e-edge-functions)
7. [Configurações Avançadas](#configurações-avançadas)
8. [Troubleshooting](#troubleshooting)

---

## 🎯 Visão Geral

Esta é uma plataforma completa de gestão de vendas, CRM e produtos de seguros com recursos avançados de automação, pagamentos e comunicação em massa.

### Principais Funcionalidades

- **CRM Completo**: Gestão de leads, oportunidades, contatos e pipeline de vendas
- **Produtos de Seguro**: Gerenciamento de produtos de seguro e assistência
- **Pagamentos**: Integração com gateways de pagamento (PIX, Cartão, Boleto)
- **Messaging em Massa**: Campanhas via WhatsApp, Email e SMS
- **Afiliados**: Sistema completo de afiliados com comissões
- **Formulários de Venda**: Criador de formulários personalizados
- **Analytics**: Dashboard com métricas e relatórios

---

## 🚀 Requisitos e Setup Inicial

### Pré-requisitos

- Conta Supabase (https://supabase.com)
- Node.js 18+ instalado
- Git instalado

### 1. Clone do Projeto

```bash
git clone [URL_DO_REPOSITORIO]
cd [NOME_DO_PROJETO]
npm install
```

### 2. Configuração do Supabase

1. Crie um novo projeto no Supabase
2. Execute as migrations do banco de dados (pasta `supabase/migrations/`)
3. Configure as variáveis de ambiente:

```env
VITE_SUPABASE_URL=https://seu-projeto.supabase.co
VITE_SUPABASE_ANON_KEY=sua-chave-anon
```

### 3. Executar Localmente

```bash
npm run dev
```

Acesse: `http://localhost:5173`

---

## 🔐 Autenticação e Usuários

### Sistema de Roles

A plataforma usa um sistema de roles baseado em níveis de permissão:

- **ADMIN**: Acesso total ao sistema
- **OPERATIONS**: Gestão operacional
- **SALES_LEAD**: Liderança de vendas
- **SALES**: Vendedor
- **FINANCE**: Financeiro
- **SUPPORT**: Suporte

### Criando o Primeiro Admin

```sql
-- 1. Primeiro, faça signup na aplicação
-- 2. Encontre seu user_id na tabela auth.users
-- 3. Execute no SQL Editor:

INSERT INTO public.user_roles (user_id, role)
VALUES ('SEU_USER_ID_AQUI', 'ADMIN');
```

### Login

1. Acesse `/auth`
2. Use email e senha cadastrados
3. O sistema redirecionará para o dashboard

### Estrutura de Permissões

Permissões são gerenciadas via RLS (Row Level Security) do Supabase:

- Cada tabela tem políticas específicas por role
- Função `has_role(user_id, role)` verifica permissões
- Função `has_permission(user_id, resource, action)` para permissões granulares

---

## 📦 Módulos Principais

### 1. Dashboard

**Rota**: `/`

Visão geral com métricas principais:
- Total de leads
- Oportunidades abertas
- Taxa de conversão
- Receita do mês
- Gráficos de performance
### 2. CRM

#### 2.1 Leads

**Rota**: `/leads`

**Funcionalidades**:
- Listagem de leads com filtros
- Criação manual de leads
- Importação em massa (CSV)
- Atribuição a vendedores
- Status: NEW, CONTACTED, QUALIFIED, LOST
- Atividades e histórico

**Criando um Lead**:
```typescript
const { data, error } = await supabase
  .from('leads')
  .insert({
    name: 'Nome do Cliente',
    email: 'email@exemplo.com',
    whatsapp: '5511999999999',
    source: 'WEBSITE',
    status: 'NEW',
    owner_user_id: 'user_id_vendedor'
  });
```

#### 2.2 Oportunidades

**Rota**: `/crm`

Sistema Kanban de gestão de oportunidades:
- Múltiplos pipelines personalizáveis
- Estágios customizáveis
- Drag & drop entre estágios
- Valores e probabilidades
- Atividades e tarefas

**Estrutura**:
- Pipeline → Stages → Opportunities

#### 2.3 Contatos

**Rota**: `/contacts`

Base unificada de contatos:
- Informações completas do cliente
- Histórico de interações
- Links com leads/oportunidades/pedidos
- Tags e segmentação
- Opt-in de marketing

### 3. Produtos

#### 3.1 Produtos de Seguro

**Rota**: `/insurance-products`

**Características**:
- Código SUSEP
- Precificação dinâmica ou fixa
- Coberturas e assistências
- Campos dinâmicos personalizados
- Declarações obrigatórias

**Tipos de Precificação**:

1. **Fixa**: Planos pré-definidos
2. **Dinâmica**: Baseada em regras (idade, capital, etc.)

#### 3.2 Produtos de Assistência

**Rota**: `/assistance-products`

Produtos complementares que podem ser vinculados a seguros.

#### 3.3 Campos Dinâmicos

**Rota**: `/dynamic-fields`

Sistema flexível de campos customizados:
- Tipos: TEXT, NUMBER, SELECT, DATE, CHECKBOX, etc.
- Aplicação global ou por produto
- Condições de exibição
- Validações customizadas

### 4. Formulários de Venda

**Rota**: `/sales-forms`

Criador de formulários de captação:

**Etapas**:
1. Dados Pessoais
2. Seleção de Produtos
3. Resumo e Pagamento
4. Confirmação

**Recursos**:
- Link único compartilhável
- Tracking de conversão
- Integração com pixels (Facebook, Google)
- Mensagens automáticas
- Afiliados

### 5. Pagamentos

**Rota**: `/payments`

#### 5.1 Gateways Suportados

- **CIELO**: Cartão, PIX, Boleto
- **Banco do Brasil PIX**: PIX recorrente

#### 5.2 Configuração

```sql
-- Exemplo: Configurar gateway CIELO
INSERT INTO payment_gateway_configs (
  gateway,
  merchant_id,
  merchant_key,
  environment,
  active
) VALUES (
  'CIELO',
  'seu_merchant_id',
  'sua_merchant_key',
  'PRODUCTION',
  true
);
```

#### 5.3 Fluxo de Pagamento

1. Cliente preenche formulário
2. Seleciona método de pagamento
3. Sistema cria charge
4. Processa via gateway
5. Webhook atualiza status
6. Notifica cliente
7. Cria invoice se aprovado

#### 5.4 Dunning (Cobrança Recorrente)

Sistema automático de retry de pagamentos:
- Múltiplas tentativas configuráveis
- Canais: WhatsApp, Email, SMS
- Janela de silêncio (horário nobre)
- Ofertas de renegociação

### 6. Afiliados

**Rota**: `/affiliates`

Sistema completo de afiliação:

**Estrutura**:
- Afiliado → Links → Conversões → Comissões

**Fluxo**:
1. Criar afiliado
2. Gerar link único
3. Rastrear cliques e conversões
4. Calcular comissões
5. Processar pagamentos

**Comissões**:
- Percentual por produto
- Status: PENDING, APPROVED, PAID
- Agrupamento em payouts

### 7. Messaging em Massa

**Rota**: `/bulk-messaging`

Campanhas de comunicação em larga escala:

**Canais**:
- WhatsApp (via Evolution API)
- Email (SMTP)
- SMS (via provedores)

**Wizard**:
1. Informações básicas
2. Seleção de canais
3. Template e variáveis
4. Audiência (upload CSV ou filtros)
5. Agendamento
6. Revisão e envio

**Recursos**:
- TPS (transações por segundo) configurável
- Janela de silêncio
- Supressão list
- Estatísticas em tempo real
- Custo estimado

---

## 🔌 Integrações

### 1. Evolution API (WhatsApp)

Sistema de multi-instâncias WhatsApp:

**Setup**:
```sql
INSERT INTO evolution_configs (
  base_url,
  global_api_key,
  webhook_enabled,
  active
) VALUES (
  'https://sua-evolution-api.com',
  'sua-api-key',
  true,
  true
);
```

**Criar Instância**:
1. Acesse `/evolution-api`
2. Clique em "Nova Instância"
3. Escaneie o QR Code
4. Configure webhooks

**Enviar Mensagem**:
```typescript
const { data, error } = await supabase.functions.invoke('evolution-send-message', {
  body: {
    type: 'text',
    instanceId: 'instance-id',
    to: '5511999999999',
    params: {
      text: 'Sua mensagem aqui'
    }
  }
});
```

### 2. ERP (MasterClassic)

Sincronização de pedidos com ERP:

**Configuração**:
```sql
INSERT INTO erp_configs (
  base_url,
  token_url,
  username,
  password,
  auto_sync,
  sync_on_order_create,
  active
) VALUES (
  'https://erp-url.com/api',
  'https://erp-url.com/token',
  'usuario',
  'senha',
  true,
  true,
  true
);
```

**Sincronização Manual**:
```typescript
await supabase.functions.invoke('sync-erp', {
  body: {
    orderId: 'order-uuid',
    action: 'CREATE'
  }
});
```

### 3. Pixels de Tracking

#### Facebook Pixel

**Configuração**:
```sql
INSERT INTO facebook_pixel_configs (
  pixel_id,
  access_token,
  active
) VALUES (
  'seu-pixel-id',
  'seu-access-token',
  true
);
```

#### Google Analytics

**Configuração**:
```sql
INSERT INTO google_analytics_configs (
  measurement_id,
  active
) VALUES (
  'G-XXXXXXXXXX',
  true
);
```

**Eventos Rastreados**:
- `page_view`
- `form_start`
- `product_select`
- `add_payment_info`
- `purchase`
- `lead_conversion`

---

## 🛠 APIs e Edge Functions

### Funções Disponíveis

#### 1. `send-whatsapp`

Envia mensagens via WhatsApp.

**Payload**:
```json
{
  "number": "5511999999999",
  "body": "Sua mensagem",
  "userId": "optional",
  "queueId": "optional",
  "sendSignature": true,
  "closeTicket": false
}
```

#### 2. `process-order-payment`

Processa pagamento de pedido.

**Payload**:
```json
{
  "orderId": "uuid",
  "method": "PIX|CREDIT_CARD|BOLETO",
  "gateway": "CIELO",
  "cardData": { ... } // se cartão
}
```

#### 3. `resolve-dynamic-fields`

Resolve campos dinâmicos por produto.

**Payload**:
```json
{
  "productIds": ["uuid1", "uuid2"],
  "formData": { ... }
}
```

#### 4. `schedule-remarketing`

Agenda tarefas de remarketing.

**Payload**:
```json
{
  "leadId": "uuid",
  "type": "FOLLOW_UP",
  "delayMinutes": 1440
}
```

#### 5. `send-form-message`

Envia mensagens configuradas no formulário.

**Payload**:
```json
{
  "salesFormId": "uuid",
  "stage": 1,
  "contactData": { ... }
}
```

### Webhooks

#### Payment Webhook

**Endpoint**: `/functions/v1/handle-payment-webhook`

**Payload Esperado**:
```json
{
  "charge_id": "uuid",
  "status": "PAID|FAILED|PENDING",
  "external_id": "gateway-transaction-id",
  "metadata": { ... }
}
```

#### PIX Webhook

**Endpoint**: `/functions/v1/pix-webhook-handler`

**Payload**: Conforme gateway (Cielo, BB)

#### Evolution Webhook

**Endpoint**: `/functions/v1/evolution-webhook-receiver`

**Eventos**:
- `connection.update`
- `qrcode.updated`
- `messages.upsert`

---

## ⚙️ Configurações Avançadas

### 1. RLS (Row Level Security)

Todas as tabelas principais têm RLS habilitado:

**Exemplo de Policy**:
```sql
CREATE POLICY "Users can view their leads"
ON leads FOR SELECT
TO authenticated
USING (
  owner_user_id = auth.uid() OR
  has_role(auth.uid(), 'ADMIN') OR
  has_role(auth.uid(), 'SALES_LEAD')
);
```

### 2. Triggers

#### Auto-criar Contato de Lead

```sql
CREATE TRIGGER create_contact_from_lead
  AFTER INSERT ON leads
  FOR EACH ROW
  EXECUTE FUNCTION create_contact_from_lead();
```

### 3. Storage

#### Bucket de Declarações

```sql
INSERT INTO storage.buckets (id, name, public)
VALUES ('declaration-pdfs', 'declaration-pdfs', true);
```

**Upload**:
```typescript
const { data, error } = await supabase.storage
  .from('declaration-pdfs')
  .upload(`${uuid}.pdf`, pdfFile);
```

### 4. Realtime

Habilitar atualizações em tempo real:

```sql
ALTER TABLE leads REPLICA IDENTITY FULL;
ALTER PUBLICATION supabase_realtime ADD TABLE leads;
```

**Subscrever**:
```typescript
const channel = supabase
  .channel('leads-changes')
  .on('postgres_changes', {
    event: 'INSERT',
    schema: 'public',
    table: 'leads'
  }, (payload) => {
    console.log('Novo lead:', payload);
  })
  .subscribe();
```

---

## 🔍 Troubleshooting

### Problemas Comuns

#### 1. Erro de Permissão (RLS)

**Sintoma**: `new row violates row-level security policy`

**Solução**:
- Verificar se usuário está autenticado
- Conferir se role está atribuído
- Validar policies da tabela

#### 2. Webhook não Recebe Eventos

**Sintoma**: Pagamentos não atualizam

**Solução**:
- Verificar URL do webhook no gateway
- Conferir logs da função (`/functions/[nome]/logs`)
- Validar assinatura se habilitado

#### 3. Evolution API não Conecta

**Sintoma**: QR Code não aparece

**Solução**:
- Verificar URL base da Evolution
- Conferir API key
- Checar logs de rede

#### 4. Formulário não Salva

**Sintoma**: Erro ao submeter formulário

**Solução**:
- Abrir console do navegador
- Verificar campos obrigatórios
- Validar formato de dados

### Logs Úteis

```sql
-- Ver últimas atividades
SELECT * FROM audit_logs
ORDER BY created_at DESC
LIMIT 50;

-- Ver charges recentes
SELECT * FROM charges
ORDER BY created_at DESC
LIMIT 20;

-- Ver campanhas ativas
SELECT * FROM bulk_campaigns
WHERE status = 'RUNNING';

-- Ver instâncias Evolution
SELECT * FROM evolution_instances
WHERE status = 'CONNECTED';
```

---

## 📊 Métricas e Analytics

### Dashboard Principal

- **Leads Today**: Total de leads criados hoje
- **Open Opportunities**: Oportunidades em aberto
- **Conversion Rate**: Taxa de conversão lead → oportunidade
- **Monthly Revenue**: Receita do mês
- **Pipeline Value**: Valor total no pipeline

### Relatórios Disponíveis

1. **Funil de Vendas**: Conversão por estágio
2. **Performance de Vendedores**: Ranking e metas
3. **Produtos Mais Vendidos**: Top sellers
4. **Campanhas**: ROI e engajamento
5. **Afiliados**: Comissões e conversões

---

## 🔒 Segurança

### Boas Práticas

1. **Nunca expor secrets no frontend**
2. **Usar RLS em todas as tabelas**
3. **Validar dados no backend (Edge Functions)**
4. **Sanitizar inputs do usuário**
5. **Usar HTTPS em produção**
6. **Rotacionar API keys regularmente**
7. **Auditar acessos (tabela audit_logs)**

### Secrets do Supabase

Gerenciar via dashboard:
- `Settings` → `Edge Functions` → `Secrets`

Usar em functions:
```typescript
const apiKey = Deno.env.get('MY_SECRET_KEY');
```

---

## 📱 Deployment

### Build de Produção

```bash
npm run build
```

### Deploy no Netlify

```bash
netlify deploy --prod
```

### Deploy no Vercel

```bash
vercel --prod
```

### Configurar Domínio Customizado

1. Acessar settings do projeto Lovable
2. Ir em `Domains`
3. Adicionar domínio
4. Configurar DNS (CNAME)

---

## 🆘 Suporte

### Recursos

- **Documentação Supabase**: https://supabase.com/docs
- **Documentação Lovable**: https://docs.lovable.dev
- **React Query**: https://tanstack.com/query
- **Tailwind CSS**: https://tailwindcss.com

### Contatos

- Email: suporte@seudominio.com
- Discord: [Link da comunidade]
- GitHub Issues: [Link do repo]

---

## 📝 Changelog

### v1.0.0 (2025)
- ✅ Sistema de CRM completo
- ✅ Gestão de produtos de seguro
- ✅ Integração de pagamentos
- ✅ Messaging em massa
- ✅ Sistema de afiliados
- ✅ Formulários de venda
- ✅ Evolution API (WhatsApp)

---

## 📄 Licença

[Definir conforme seu projeto]

---

**Última atualização**: Janeiro 2025
