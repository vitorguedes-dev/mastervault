Data: 10/09/2025
Autor: Vitor Hugo
Objetivo: Auxílio e concentração de informações para uso do fluxo da Jornada 3

---
## Informações Básicas

**Nome do Workflow:** Jornada 3 - Produção
**URL Webhook:** https://webhook.flux.orkai.com.br/webhook/jornada3-prod
### Dados a serem enviados
Formato e estrutura dos dados em JSON a serem enviados para o Webhook:

```json
{
    "cpfDevedor": "06707357603",            // 11 dígitos obrigatório
    "nomeDevedor": "HECTOR DUTRA",         // <= 140 chars obrigatório
    "contrato": "C-000999",                 // <= 35 chars obrigatório
    "dataInicial": "2025-09-11",            // YYYY-MM-DD >= hoje
    "periodicidade": "MENSAL",              // DIARIA|SEMANAL|MENSAL|ANUAL
    "politicaRetentativa": "PERMITE_3R_7D", // Ver opções na seção 5.1 Do Guia Técnico
    "valorRec": "0.01",                     // Recomendado para valor fixo
    
    // Específicos da cobrança imediata
    "valorPrimeiroPagamento": "0.02",       // Valor do pagamento imediato
    "chavePixRecebedor": "02429647000169",  // Chave Pix do recebedor
    "nomeRecebedor": "VIDA OURO",           // Opcional para logs
    "cidadeRecebedor": "BELO HORIZONTE"     // Opcional para logs

}
```

### Resposta esperada - 200 OK

```json
{
    "qrcode": "iVBORw0KGgoAAAANSUhEUgAAASwAAAEsAQMAAABDsxw2AAAABlBMVEX///8AAABVwtN+AAAACXBIWXMAAA7EAAAOxAGVKw4bAAAEvElEQVRogd1aMbKjShBrioBwjjA3gYtRD1y+GNyEIxASUPSX1Pj93WzDNg5262E5aHePWtLY7N9ek7sf5pdZuWza8aSaHY1fZSlX8a3F+76lho144xh8wX8/5e1nbbdu69YDnyovf2/tZtYlh1XfUOlS3Gd9xgGzBuWtPrPSzf0bYEN3WY9WtXvrAKJ76B2aZe3XwIxDtVs5jU8BG9C/q8zPgmksjz4oYG+3E1OIKW3QU99H+2t6c8JEDkO37n157WN5b2fttqP/PKjvPzgkKSxe4IJ1n5zNYqXWd0sBX5S5zvbHKycMlWKijp4w9kaFdSvYQhQtwjseAjtqxym88HTG2TpRfnd1jv3U+Fzfm9XkMJAD9+nq3D1nOatgXEboqd61GhySFqbDv4DJB3DBhNLIBXiwqHvTNmI7eXKYsxmo03f0BkdJ+5Qaode0zXaq0sQwtQJKbG9YGGrlA1S64lPYp87e+SNgYzWMHuofsE+xU2urTx0iP9KgBF1q2MSFj2XEocQy8k+lIavbbfpwSF4Y65AkGKRqQoT6xWV079PWRQ6JYdCcnUuHub/BBuICE2zGNM4mSfAAGHsarg1NpNI5ISMgfsghLyeliENyw45qw0HF+YJbeLu4oMc+XXcuo5M+1FLDWp4cLqKGq0i1QiPAARUZuQ85ZIaNmDYbwu2L00R36zHsPcw0YcwDngDj8+AQf+FkjbWlBIWgg1+gb6OrSw5r5aedkpnkTcUZTP7xCxrM1DCeLFRqYPIGw0mz3NE/czDpncdoVWbYqMLgZS58YrZJbt+XjirByNqwcVI1eWGm9CKaBVVD5Q/gInp47To/auf3w0L8GC02mYKaE98QHRtEKKidYjs7bGOwxnBm/ykgB9mDyDuLttMWJys5jBJ0x8kCaFKlUjXMOzmpGsvMMG4ZGYFG23Su0mFki177lH92D4FxXwoK7Ua/wC+k72LB8hu6125mmIeGZnrO/BwHiZ+7YsHC8pyWHkZyEBPQO5sFOXT83MXopmrjZoahGQyZrdyJWb2zGt9Z6VzffofMeWFKJQ2VDrcRoP23XnJzjgAtvPPXw6aN6o2BrHH3xCWaHNCiVOod5JAZxnsXHiQFa9RvtDy60IhlpHNlqWEjrbIxZDaerVa5BoMOUvTOpEbtzA2jDhM5/JSz8PrVdbJ0O6bItj4DxpI5lvg6YHhuvyDYUpTrOtOc5DB6Z7Rw4CSCCdBT5yWaf1KCW4Jmhh2s9PdSg/Zflmfv1awP4SeGMc/s/K4Uur9q3hhvrjJxFnHTA2D8x8OfcjBnJbtcRaCVWLtnmOy8sBA/jNP5ouUxWez1Nxs8a/iFvLCqbFC5BsnBeZBwthg3URLMiqFTw6awLo1ukkYbZZWPBt2jvQGs3vY/L0wTN2h54ql+z+B3yBzXgrcTfQbMAGMEFT/V4FiuR39rb15qJIexNFQKYmALdZTC8mhOeft/Rzp5YfrNT/zaRPpNlR7hnclvDDqywyrjprCezu7c80bj6RH6kbOfA+OvTV5UcOgr3qF+c2hS3ghocPPDPrnG/xI0fufw5g8zPTss+O2++2dCWzWWy+cSzez3ZCWFBTnchdFl6lLjljiKOO97ma+H/dvrP641pq8rpqO8AAAAAElFTkSuQmCC",
    "dadosQR": {
        "jornada": "JORNADA_3",
        "pixCopiaECola": "00020101021226850014br.gov.bcb.pix2563qrcodepix.bb.com.br/pix/v2/733b9e99-37cd-4d67-9238-20438d830a885204000053039865802BR5925CLUBE DE VANTAGENS VIDA O6008Brasilia62070503***80850014br.gov.bcb.pix2563qrcodepix.bb.com.br/pix/v2/rec/05336c8df5814af187985bd469387a3863040C2D"
    },
    "vinculo": {
        "devedor": {
            "nome": "HECTOR DUTRA",
            "cpf": "06707357603"
        }
    }
}
```

**OBS: O QRCode está em formato base64**

### Atenção
Este é o ambiente de produção em vigência, como mencionado em e-mails anteriores, o ambiente de homologação funciona com dados fixos descritos pela API, no ambiente de produção é possível incluir dados reais, mas esses dados e a recorrência serão cadastrados e incluídos no extrato de cobranças do BB, se confirmados.