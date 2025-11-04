Data: 24/10/2025
Autor: Vitor Hugo
Objetivo: Demonstrar o sistema pix automático e como buscamos integrar aos nossos sistemas

---
## Pix e Pix Automático - Recorrente
Todos já conhecemos e utilizamos o pix para transferências e pagamentos pontuais. Você já usa manualmente através de qualquer instituição financeira mas você precisa realizar manualmente ou só realiza um pagamento por vez.

O Pix Automático é uma alternativa de cobrança. Criada pelo GOV BR, ou seja, será possível realizar cobranças automáticas diretamente na conta da pessoa após a autorização de forma segura pelo sistema Pix.
### Como isso acontece?
Através de instituições autorizadas, como o Banco do Brasil (Primeira instituição a ser autorizada a utilizar o sistema), é possível criar um sistema automatizado de cobrança recorrente através do Pix. 

Como é possível? Utilizando a API disponibilizada pelo Banco do Brasil e seguindo as regras definidas geramos uma recorrência e o cliente autoriza a cobrança continuada (Diária, mensal, anual ou semanal) diretamente na conta bancária.
### API do Banco do Brasil
O banco do Brasil disponibiliza e específica as regras para acessarmos o sistema Pix e utilizar as funções do Pix Automático. Por meio da documentação, o BB define o modelo de uso da API dividido em quatro jornadas.
#### Jornada 1 - Notificação via APP
Somente a autorização da recorrência do cliente, não necessita o QR Code gerado. É apenas a notificação push pelo banco do cliente.

- Ótimo para autorizações em massa.
- O cliente só abre e realiza a confirmação.