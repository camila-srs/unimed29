Nessa página vamos descrever os parâmetros padrões do sistema que foram consultados e/ou alterados para atender as regras de contabilização de acordo com a necessidade da nossa área de contabilidade.

[Link TDN](https://centraldeatendimento.totvs.com/hc/pt-br/articles/360000227308-MP-SIGAFIN-CTBAFIN-Performance-na-Contabiliza%C3%A7%C3%A3o-do-Financeiro)

No configurador, ajuste as configurações de parâmetros abaixo:

*  MV_FINLOG precisa ser habilitado para .T. - http://tdn.totvs.com/pages/viewpage.action?pageId=6070492

*  MV_DIRDOC precisa ser definido para a geração do arquivo de inconsistências (verificar se está apontando para uma pasta valida do ERP ex. System)

*  MV_PRELAN precisa estar como “D” - http://tdn.totvs.com/pages/viewpage.action?pageId=284348618

*  MV_ATUSAL precisa estar como “S”

*  MV_CFINTHR pode colocar a quantidade que o SLAVE aguentar (limitado a 30 threads) - http://tdn.totvs.com/pages/viewpage.action?pageId=6070318

*  MV_CTBUPRC deve estar como .F. - http://tdn.totvs.com/pages/releaseview.action?pageId=223930616

*  MV_CTBFLAG deve estar como .T. - http://tdn.totvs.com/pages/viewpage.action?pageId=185757542
