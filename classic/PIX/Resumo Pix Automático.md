Data: 04/09/2025

### Erros Atuais - Jornadas 2 e 3

- **Chaves Pix não autenticadas no BB** - Provavelmente devido ao ambiente de homologação (Sem certificado) utilizar dados de recebedor padrão.
- **Acesso a criação de cobranças não autorizado** - Mesmo com o token gerado com todas as permissões, além de registrado no sandbox, o retorno é 403 - acesso não autorizado. Atualmente, foi reportado ao suporte do BB e não há motivos aparentes.
- **Recorrências cadastradas não encontradas** - Recorrências que geradas pela API retornam não encontradas na chamada da URL de criação da cobrança. 
- **Erro 400 vazio** - Sim
- **QRCode e código copia e cola não funcionais** - São gerados pela API e não funcionam corretamente para pagamento além de serem gerados com recebedor diferente do CNPJ da Aplicação (empresa de construção) -> Provavelmente é o ambiente de homologação utilizando dados padrões

### Erros Recorrentes

- **Erro 504 Gateway Time-out** - API não responde a solicitação da requisição
- **Erro 500 DNS não encontrado** - DNS da API está fora do ar