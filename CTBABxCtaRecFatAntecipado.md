![image](uploads/8a84ff499296cede9fdb2c420b6c1b60/image.png)
Nesta página serão descritas as regras de contabilização do faturamento antecipado. 

O que é um faturamento antecipado? segundo orientações obtidas junto a nossa contabilidade, refere-se aos períodos de coberturas. Por exemplo o contrato possui inicio de cobertura dia 15, então o seu período é de 15/x à  14/x+1. Os registros contábeis são feitos de 01 a 30 de cada mês. Logo entende-se que o período do mês seguinte, recebido dentro do mês "x" é um faturamento antecipado.

**01 - Regras para faturamento antecipado**

**01.1** - No ERP foram criados dois campos para guardar a quantidade de dias de cada período, ou seja, cada fatura possui no máximo dois períodos. A customização foi feita na tabela padrão do sistema ( `SEV->EV_XDIAS1P` ) e ( `SEV->EV_XDIAS2P` ), eles estão com os nomes "Qd. Dias 1P" e  "Qd. Dias 2P".

**REGRA 01** - Caso o "Qd. Dias 2P" ( `SEV->EV_XDIAS2P` ) for menor igual a zero, não seguirá regra de faturamento antecipado.

**REGRA 02** - Caso o "Qd. Dias 1P" e  "Qd. Dias 2P" ( `SEV->EV_XDIAS1P` ) e ( `SEV->EV_XDIAS2P` ), estiverem iguais a zero, não seguirá a regra de faturamento antecipado.

**REGRA 03** - O numero do lote onde serão gerados os lançamentos contábeis que correspondem ao faturamento antecipado será ???? ( Leonardo - Contabilidade definir ??? )

**REGRA 04** - A rotina que será executada no último dia de cada mês ( `LASTDAY()` ) deverá observar as seguintes condições.

**04.1** - Na tabela de controle de arquivos "competências" ( SZ6030 ), fazer a busca se há registro do arquivo com extensão ".FAN" na data que corresponde ao último dia do mês do qual está fazendo o processamento. Por exemplo, caso esteja sendo executado no final do mês de Setembro/2019, então fará a procura por registro 20190930.FAN. Se encontrar, é porque já houve o processamento e não deve fazer nada.

Caso não encontre deverá realizar a geração dos lançamentos contábeis conforme as regras e escrever o dado nesse tabela.

**REGRA 05** - Data da execução da rotina deve seguir a seguinte regra; após a virada do mês deve observar o parâmetro MV_DATAFIN ( parâmetro do modulo configurador onde o setor de contabilidade controla a quantidade de dias tolerável para receber movimentações do setor financeiro ). Normalmente são 5 dias úteis.

A execução deve ocorrer no dia 10 do mês seguinte ao período e deve consultar o parâmetro MV_DATAFIN se está com data maior ao último dia do mês anterior. Caso não esteja, não deve processar a rotina de lançamentos, apenas enviar e-mail para lista de pessoas responsáveis pela área contábil informando que a rotina não foi executada, e no melhor momento isso deve ser feito de forma manual.

Exemplo de variáveis para esta regra:

Período a ser observado : 01/09/2019 a 30/09/2019
Data geração lctos      : 30/09/2019
Data do processamento   : 10/10/2019
Arquivo na SZ6030       : 20190930.FAN
MV_DATAFIN              : ( Se data > 30/09/2019, executa rotina, senão envia e-mail )

**REGRA 06** - Período das baixas a ser verificado para aplicação das regras, compreende baixas de faturas/títulos que tiveram origem no SGU 2.0 entre o dia 01 e ultimo dia do mês (pode ser 28,29,30 e 31 depende do mês ). Por exemplo 01/09/2019 a 30/09/2019.

**REGRA 07** - Reversão do faturamento antecipado ( 1º período )

Exemplo: Contrato com vigência 15/x a 14/x+1, esta regra fará a geração dos lançamentos contábeis referente ao período 15/x a 30/x, a data do lançamento contábil será  no dia 30/x

O valor a ser considerado para os lançamentos contábeis, é valor proporcional dos dias referente ao primeiro período com relação ao valor bruto da fatura. Esse valor bruto da fatura está na tabela ( `SEV->EV_VALOR` ), a quantidade de dias do período também está nessa tabela no campo ( `SEV->EV_XDIAS1P` ).

Para obter as contas contábeis da reversão, posicionar no cadastro de centro de custos: 

CONTA DÉBITO  - utilizar o conteúdo "Conta CTB 01" ( `CTT->CTT_XCTA1` ) 

CONTA CRÉDITO - utilizar o conteúdo "Conta CTB 02" ( `CTT->CTT_XCTA2` ).


**REGRA 08** - Reversão do faturamento antecipado ( 2º período )

Exemplo: Contrato com vigência 15/x a 14/x+1, esta regra fará a geração dos lançamentos contábeis referente ao período 01/x+1 a 14/x+1, a data do lançamento contábil será  no dia 30/x+1

O valor a ser considerado para os lançamentos contábeis, é valor proporcional dos dias referente ao segunda período com relação ao valor bruto da fatura. Esse valor bruto da fatura está na tabela ( `SEV->EV_VALOR` ), a quantidade de dias do período também está nessa tabela no campo ( `SEV->EV_XDIAS2P` ).

Para obter as contas contábeis da reversão, posicionar no cadastro de centro de custos: 

CONTA DÉBITO  - utilizar o conteúdo "Conta CTB 01" ( `CTT->CTT_XCTA1` ) 

CONTA CRÉDITO - utilizar o conteúdo "Conta CTB 02" ( `CTT->CTT_XCTA2` ).

Situações a serem testadas:
---

Bom dia!

Roberto,
Consultei algumas das faturas e constatamos as seguintes operações;

1)	Recebimento parcial antes do vencimento; o sistema aplica a proporção conforme a vigência do mês que foi faturado, sendo uma parte dos valores baixado das contas originais (emissão da fatura) e a outra em recebimentos antecipados (fatura 14424733), segundo recebimento na data de vencimento, o sistema aplica a proporção conforme a vigência do mês que foi faturado, sendo uma parte dos valores baixado das contas originais (emissão da fatura) e a outra em recebimentos antecipados.

2)	Recebimento parcial após o vencimento; o sistema aplica a proporção conforme a vigência do mês que foi faturado, sendo uma parte dos valores baixado das contas originais (emissão da fatura) e a outra em recebimentos antecipados (fatura 14455378), segundo recebimento após a data de vencimento, o sistema aplica a proporção conforme a vigência do mês que foi faturado, sendo uma parte dos valores baixado das contas originais (emissão da fatura) e a outra em recebimentos antecipados.

3)	Recebimento integral após o vencimento, mas ainda dentro do período de cobertura, o sistema aplica a proporção do período de cobertura e baixa os valores, sendo uma parte dos valores baixado das contas originais (emissão da fatura) e a outra em recebimentos antecipados (não localizei um exemplo).

4)	Recebimento integral após o vencimento e após o período de cobertura do contrato; o sistema baixa 100%  nas contas originais de faturamento (14438376).


Avaliando o contexto e as faturas, identificamos que o sistema respeita o período de cobertura da fatura gerada, para qualquer situação de pagamento, independente se o recebimento for realizado parcial ou integral, antes ou após o vencimento, o sistema irá identificar a proporção da cobertura no ato do recebimento e aplica as proporções de valores e contas contábeis.


Atenciosamente,

Leonardo Soares
Assistente de Processos
Controladoria / CRC-19054
(62) 3216 8089

