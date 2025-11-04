
Boa tarde,

  

Foi confirmado que o ambiente de homologação da API do BB só funciona com base em dados fixos definidos pelo Time do BB, descritos na API. Para confirmação completa da criação e ativação da recorrência é necessário criar um alerta baseado em webhook para consultar a recorrência criada nos horários definidos pela API  
  
Dessa forma, os testes para confirmar se o QRCode é funcional não são efetivos utilizando o ambiente de homologação, visto que, os códigos do pix copia e cola são criados com base nos dados fictícios e fixos da API.

  

No momento atual, 08/09/2025, estou aguardando a ativação do ambiente de produção da aplicação (conforme imagem abaixo) que já está autenticada com os certificados para iniciar os testes efetivos e com dados reais. Será necessário solicitar ao suporte BB a ativação do ambiente de produção visto que eles não definem prazo para este processo.  
​![image.png](blob:https://mail.google.com/7ecdd245-0b88-43a6-9687-6c084b028621)​  

  

**PROGRESSO:**

  

Após o estudo aprofundado da API, foi realizada a criação de um **guia técnico de implementação (em anexo)** listando os pontos de atenção, dados obrigatórios de cada etapa, descrição de todas as jornadas e instruções de implementação. 

  

Após a criação desse documento foi identificado diversos pontos de complexidade não identificados no primeiro momento, ou seja, não basta apenas seguir as jornadas, é preciso criar um sistema de integração completo devido a complexidade adicional.

  

O estudo da integração segue em andamento, será criado um projeto no CRM para organização e divisão de tarefas do mesmo, assim como, fluxogramas para entendimento dos fluxos a serem criados.

  

Qualquer dúvida estou à disposição!