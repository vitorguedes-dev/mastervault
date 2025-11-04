Data: 31/10/2025
Autor: Vitor Hugo

**Contexto:** Após a reunião com a gerente do Banco do Brasil, foi identificado reavaliar os objetivos do sistema Pix Automático para isso é necessário que os gestores definam as questões abaixo.

---
## Gestores - Áreas relacionadas
- Quais as responsabilidades do Parceiro em relação a cobrança? Apenas recebimento de comissão? Fará a cobrança de inadimplência?  **(Ramael, Dani)**
    - Quais as responsabilidades da Tapaggo em relação a isso? Controle de repasse de comissão (Split de Pagamento)? Cobrança de Inadimplência? Emissão e envio de relatório de cobrança e inadimplência ao parceiro? **(Ramael, Dani)**
    
- Se o cliente cancelou a recorrência, o que fazer? Realizar retenção/contato com o cliente? Enviar relatório ao parceiro?  **(Ramael, Dani e Hector)**
    
- Se uma das cobranças recorrentes falhar, o que fazer? Definir qual política de retentativa de cobrança realizar ou contato direto com o cliente? Emissão de cobrança comum (QR code pix comum)? **(Ramael, Dani e Hector)**
    
- O que irá fazer com relação ao reajuste anual? E as Porcentagens? Lembrando que a jornada 3 não permite a alteração do valor pré-definido da cobrança recorrente! Na jornada 2, o cliente define o valor máximo a ser cobrado.  **(Ramael, Dani, Hector)**
    
- Como realizar a separação de pagamentos comuns oriundos da cobrança da inadimplência dos pagamentos recorrentes? Contas de recebimento distintas, conforme proposto pela gerente do BB? **(Ramael, Dani)**
    
- Como funcionará detalhadamente o Split de Pagamento? E no caso da primeira cobrança imediata (J3)?
    - O Split de Pagamento envolve outra API do BB? Qual API? Lis? **(Ramael, Hector)**
    
- Qual vai ser a volumetria dos dados e quantos parceiros utilizarão a solução? **(Ramael)**
    
- Quais informações o parceiro vai poder acessar pelo sistema dele? **(Ramael)**
    
- Quais dados são necessários para área financeira? (Relacionados somente ao Pix Automático) **(Dani)**
    
- Quais dados são necessários para a área Operacional? Considerar o Lis.  **(Hector)**
    
- A autorização é condicional ao pagamento da cobrança imediata (J3). E se ocorrer falha na autorização da recorrência? Lembrando que a cobrança imediata nesse caso já foi paga. **(Ramael, Dani, Hector)**
    - Como enviar a solicitação de autorização novamente? **(Ramael, Dani, Hector)**
        
- Como ocorrerá o tratamento das funcionalidades principais (CORE no diagrama) com o Lis? Quais funcionalidades serão solicitadas? **(Hector)**. **Link:** [Diagrama Proposto](https://www.tldraw.com/f/YREKE9Bx9_0vh-3F1nc58?d=v-229.-756.5870.2688.9rXEPGRB_bZXRT8PAh70j)

## Área de dados
É necessário que acompanhem o board que descreve as funcionalidades a serem atendidas.
**Link:** [Diagrama Proposto](https://www.tldraw.com/f/YREKE9Bx9_0vh-3F1nc58?d=v-229.-756.5870.2688.9rXEPGRB_bZXRT8PAh70j)

- Desenvolvimento do Banco de Dados de acordo com as necessidades dos sistemas integrados e dos times operacional e financeiro. **(Wender, Luyara)**

## Área de Desenvolvimento
É necessário que acompanhem o board que descreve as funcionalidades a serem atendidas.
**Link:** [Diagrama Proposto](https://www.tldraw.com/f/YREKE9Bx9_0vh-3F1nc58?d=v-229.-756.5870.2688.9rXEPGRB_bZXRT8PAh70j)

- Desenvolvimento dos sistemas de parceiros com documentação do que foi definido e do que irá ser implementado. **(Jhennifer)**
- Desenvolvimento do sistema de gestão do pix automático interno, atendendo as demandas do time financeiro. **(Matheus)**
- Desenvolvimento da API de acordo com o que for definido pelos gestores. **(Vitor)**
- Acompanhamento com o time do LIS para implementar as funcionalidades principais CORE do sistema. **(Vitor, Hector)**