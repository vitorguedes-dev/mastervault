
Considerações a cerca do formato dos dados a serem importados no CRM.

Formato do arquivo em **CSV - UTF-8**, modelo presente no caminho de pasta abaixo:
**Z:\Operacional\Tecnico\7 - Projetos\09 - Projeto CRM**

Pode ser baixado diretamente no site do CRM em importar leads -> Download sample, imagem abaixo:
![[Pasted image 20250808171154.png]]


**STATUS:** Sempre será o status **45** -> IMPORTAÇÃO

**SOURCE:** Indicar o **número** da SOURCE criada pelo importador

**NÚMERO DE TELEFONE**: +55 DDD NÚMERO -> +55319123456789
	*- OBS: Deve conter o sinal e o código do país*

**NOME**: Em maiúsculo, sem acentos -> JOSE OLIVEIRA

**CAMPO DESCRIÇÃO:** Todas as informações que não estão especificadas no cabeçalho do modelo devem ser agrupadas nesse campo

**TAGS:** São os produtos separados por vírgula (Testar se vai quebrar o modelo), **via de regra deixar em branco pois esses leads ainda não contrataram nenhum plano.**

**ATENÇÃO**
- Cabeçalhos que não tiverem informações (Zip, Ma point, etc) -> deixar em branco.
- Verificar sempre se o CRM está importando Leads em branco
- Verificar sempre se o CRM está importando campos com bugs ou com informações imprecisas.
- Verificar se o arquivo em CSV está corretamente formatado antes de importar, pode ocorrer em informações quebradas ou imprecisas.

O CRM DISPONIBILIZA UMA OPÇÃO DE **SIMULAR IMPORTAÇÃO** QUE PODE SER UTILIZADA PARA VERIFICAR AS CONSIDERAÇÕES ACIMA.

![[Pasted image 20250808172155.png]]