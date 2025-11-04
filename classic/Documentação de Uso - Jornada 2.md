Data: 09/09/2025
Autor: Vitor Hugo
Objetivo: Auxílio e concentração de informações para uso do fluxo da Jornada 2

---
## Informações Básicas

**Nome do Workflow:** Jornada 2 - Produção
**URL Webhook:** https://webhook.flux.orkai.com.br/webhook/jornada2-prod 
### Dados a serem enviados
Formato e estrutura dos dados em JSON a serem enviados para o Webhook:

```js
{

  "cpfDevedor": "06707357603",          // 11 dígitos obrigatório

  "nomeDevedor": "HECTOR DUTRA NASCIMENTO",   // <= 140 chars obrigatório

  "contrato": "C-000100",               // <= 35 chars obrigatório

  "dataInicial": "2025-12-01",          // YYYY-MM-DD >= hoje

  "periodicidade": "MENSAL",            // DIARIA|SEMANAL|MENSAL|ANUAL

  "politicaRetentativa": "NAO_PERMITE", // Ver opções na seção 5.1 do Guia Técnico

  "valor": {"valorRec": "0.01"},        // Se omitido = variável

  "convenio": "33618"                   // Opcional se só 1 convênio
}
```

---

### Resposta esperada - 200 OK

```js
{
    "success": true,
    "jornada": "2",
    "descricao": "QR de autorização (J2)",
    "dados": {
        "idRecorrencia": "RN0000000020250909BB259724582",
        "status": "CRIADA",
        "jornada": "JORNADA_2",
        "pixCopiaECola": "00020126180014br.gov.bcb.pix5204000053039865802BR5925CLUBE DE VANTAGENS VIDA O6008Brasilia62070503***80850014br.gov.bcb.pix2563qrcodepix.bb.com.br/pix/v2/rec/464b42be148245f2b6164ce1451395156304B0CA"
    }
}
```
### Atenção
Este é o ambiente de produção em vigência, como mencionado em e-mails anteriores, o ambiente de homologação funciona com dados fixos descritos pela API, no ambiente de produção é possível incluir dados reais, mas esses dados e a recorrência serão cadastrados e incluídos no extrato de cobranças do BB, se confirmados.