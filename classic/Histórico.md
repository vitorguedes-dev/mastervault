
jornada 3 insurtech
![[Pasted image 20251021145537.png]]


![[Pasted image 20251021145633.png]]


const crypto = require('crypto');
const txid = crypto.randomBytes(16).toString('hex');

const config = $node['1. Config Dados'].json;

const body = {
  calendario: {
    expiracao: 3600
  },
  valor: {
    original: config.VALOR_PRIMEIRO_PAGAMENTO
  },
  chave: config.CHAVE_PIX_RECEBEDOR,
  solicitacaoPagador: "Primeira parcela - Pix Automatico"
};

return [{
  txid: txid,
  body: body
}];







