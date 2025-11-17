data: 14/11/2025
Autor: Vitor

---
# Pix - Cenário Atual

## Objetivo Inicial
Um sistema que consiga gerar uma recorrência de pagamentos através do Pix Automático e que pode prover consultas sobre estas recorrências e as cobranças geradas, individualmente (Por Devedor) e coletivamente (Por parceiro) de maneira visual e organizada.
## Como estamos?
No momento atual, o sistema pix está funcional. O que temos agora:
- Geração do QRCode + Pix copia e cola;
- Cancelamento individual de cobranças funcional;
- Mapeamento de recorrências e cobranças por devedor (Cliente).
## O que significa funcional? 
No melhor cenário, ou seja, sem considerar alterações no processo, a autorização da recorrência e as cobranças acontecem! Funciona no melhor cenário.
## O que precisamos?
- **Reemitir cobranças;**
	 Em caso de inadimplência de alguma parcela devemos conseguir identificar e reemitir a cobrança pro devedor através do Pix Comum e registrar o acontecimento. (A solução está  direcionada para tratamento no Lis quando eles prosseguirem com a implantação do Pix).
- **Controle de Vencimento da Cobrança;**
	 Não temos controle sobre as datas de vencimentos e retentativas. (Esta etapa também está direcionada para tratamento no Lis).
- **Link do Pix Comum e do Pix Recorrente;**
	 No caso da cobrança imediata (Jornada 3 e Pix comum) não temos registro nem atrelamos a mesma com a recorrência ativada.  No caso de cobranças reemitidas também não temos como atrelar ao devedor.
- **Cobranças Liquidadas (Efetivamente efetuadas);**
	 Consequentemente não temos controle efetivo de cobranças liquidadas.
- **Consulta dos dados do Devedor/Pagador/Cliente;**
	 Busca dos dados das recorrências e do cliente no sistema Pix Automático (API do Banco do Brasil).
- **Controle dos dados do Devedor/Pagador/Cliente;**
	 Registro e mapeamento efetivo dos dados dos clientes que escolhem o Pix automático, levando em consideração os parceiros. Nem controle efetivo dos dados do Recebedor/Parceiros (Credenciais de API, Contas de plataformas, chaves pix e como utilizá-los de forma automática).
### Continua...
## Por que um Banco de Dados?
Para guardar e permitir a gestão dos dados necessários e gerados no sistema construído.
- Dados do Devedor/Pagador (Clientes).
- Dados da Recorrência.
- Dados das Cobranças efetuadas e não efetuadas
- Dados das Cobranças Liquidadas (Confirmadas).
## De onde vem os dados?
Majoritariamente dos parceiros e da API do Banco do Brasil.
