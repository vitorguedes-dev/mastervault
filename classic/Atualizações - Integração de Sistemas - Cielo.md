
Bom dia,
  
A integração com a Cielo está funcional, foram realizados alguns testes pontuais e segue em período de testes para identificar falhas e casos especiais a serem tratados.  

Agora, a integração cobre:
- Identificação de terceiro pagador
- Controle das informações recebidas da Cielo.

Futuras implementações:
- Consulta da transação diretamente na API da Cielo
- Criação de links de pagamento por API

Para tal é necessário avaliar o tempo disposto, necessidade e as alterações necessárias a seguir:

É necessário realizar as seguintes alterações:  
   - Tratamento de erros - Necessário tratar os casos de erro identificados nos testes pra integração completa e íntegra.
   - Validação de Dados -  A integração depende da integridade dos dados para ter sucesso e não haver falha ou perda no armazenamento.