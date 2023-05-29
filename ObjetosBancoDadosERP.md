**Objetos de banco de dados**
---

**01 - SCHEMA: `SIGA`**
---

Este é o usuário do qual o sistema Protheus TOTVS utiliza no banco de dados para construir os seus objetos, é o usuário padrão do sistema, possui permissão de DBA. Está configurado na camada intermediária chamada DBAcess. Nele foram configurados/customizados os seguintes objetos para integração:

| **VIEW**| **Objetivo**|
| ------ | ------ |
|V_FATURA_REC_ITEM|Obter dados complementares de faturas geradas no SGU 2.0|
|V_INTGRA_RET_ERP|Escrever dados para serem integrados com SGU 2.0|
|V_INTGRA_SGU_ERP|Leitura de dados do SGU 2.0 para ser integrado com ERP|
|V_INTGRA_SGU_ERP_ITEM|Leitura de dados do SGU 2.0 complmentar a view V_INTGRA_SGU_ERP|
|V_LOTE_CTABIL| Obter dados complementares de lotes contabeis|
|V_MVTO_REC_TAXA|Obter dados complementares de faturas geradas no SGU 2.0|
|V_PARAM_VALOR|Obter dados complementares de parâmetros no SGU 2.0|

**02 - SCHEMA: `SGU`**
---

Esse usuário foi criado exclusivamente para manter os objetos de banco de dados que são utilizados na integração do SGU 2.0 para o ERP e vice versa. Os objetos criados são:

**02.1 - Databaselink (dblink)**

| **Ambiente** | **dblink** |
| ------ | ------ |
| Produção | @dbsgu |
| Homologação | @desvsgu | 

**02.2 - Tabelas :**

| **Nome**| **Objetivo**|
| ------ | ------ |
|INTGRA_RET_ERP| Fila de dados para integrar os dados do ERP com SGU 2.0 |
|INTGRA_SGU_ERP_DEB_AUTM | Manter dados de beneficiários do débito automático do SGU|
|W_CRITICA_PROCESSO | Mantém criticas de processos de integração |
|HIST_DEB_AUTM_EMAIL| Mantém histórico envio de e-mails |


**02.3 - Sequence**

| **Nome**| **Objetivo**|
| ------ | ------ |
|S_INTGRA_RET_ERP | Sequence utilizada na tabela SGU.INTGRA_RET_ERP |

**02.4 - Views**

| **VIEW**| **Objetivo**|
| ------ | ------ |
|V_CADAST_MSGS_ITEM| Obter dados das mensagens de faturas no SGU 2.0 |
|V_EMIS_FATURA_REC_BASE| Obter dados complementar das faturas no SGU 2.0 |
|V_FATURA_REC| Obter dados das faturas no SGU 2.0 |
|V_FATURA_REC_ITEM| Obter dados complementares de faturas geradas no SGU 2.0|
|V_GYN_CNTRAT_CANCELADO|Obter dados referente aos contratos cancelados por inadimplência no SGU 2.0 (Faturas PF e PJ)|
|V_INTGRA_RET_ERP|Escrever dados na tabela de integração ERP para SGU 2.0|
|V_INTGRA_SGU_ERP|Obter e atualizar dados na tabela de integração SGU 2.0 para ERP|
|V_INTGRA_SGU_ERP_ITEM|Obter dados na tabela ITEM de integração SGU 2.0 para ERP|
|V_LOTE_CTABIL| Obter dados complementares referente ao lote contabil no SGU 2.0|
|V_MVTO_REC_TAXA|Obter dados complementares de faturas geradas no SGU 2.0|
|V_PARAM_VALOR|Obter dados complementares de parâmetros no SGU 2.0|
|V_TITULOS_VENCIDOS|Obter dados sobre títulos vencidos no SGU 2.0 (api-uniweb)|



**02.5 - Procedures:**

| **Nome**| **Objetivo**|
| ------ | ------ |
|P_INTGRA_DEB_AUTM_ERP_SGU| Envia dados débito automático para SGU 2.0 |
|P_INTGRA_ERP| Envia dados da tabela INTGRA_RET_ERP para SGU 2.0 |
|P_INTGRA_SGU_ERP_DEB_AUTM | Mantém dados referente ao débito automático na tabela INTGRA_SGU_ERP_DEB_AUTM |


**02.6 - Funções:**

| **Nome**| **Objetivo**|
| ------ | ------ |
|CALC_DV_MOD11BIOMEEK| Utilizada para ID débito automático do BIOMEEK |
|CALCULA_DV_MODULO11| Utilizada criação e/ou validação dos códigos de identificação do débito automático|

**02.7 - JOBs:**

| **Nome**| **Objetivo**|
| ------ | ------ |
|J_P_INTGRA_DEB_AUTM_ERP_SGU| Utilizado para enviar dados débito automático ERP para SGU 2.0|
|J_P_INTGRA_SGU_ERP_DEB_AUTM| Utilizado para executar a procedures P_INTGRA_SGU_ERP_DEB_AUTM|
|J_P_INTGRA_ERP| Utilizado para executar a procedure P_INTEGRA_ERP|


**Configurações no ERP**
---

**01 - Binários**

Foi criado um ambiente no ERP ( Protheus ) para atender as integrações automáticas, para isso foi utilizado um "appserver" exclusivo com o nome "appserver_wsjob", com o arquivo de configuração "appserver.ini" com os seguintes dados:

``````
[APO_WSJOB]
SourcePath=C:\ERP_PROTHEUS\Homologacao\rpo\apo_wsjob
RootPath=C:\ERP_PROTHEUS\Homologacao\protheus_data
StartPath=\system\
x2_path=
RpoDb=top
RpoLanguage=portuguese
RpoVersion=120
LocalFiles=ctree
Trace=0
localdbextension=.dtc
PictFormat=DEFAULT
DateFormat=DEFAULT
HelpServer=10.64.60.51:8079
MaxLocks=50000
Theme=Sunset
;BuildKillUsers=1
TOPMemoMega=1
;LogProfiler=1
;IXBLOG=NORUN

[TopConnect]
Alias=desvsiga
DataBase=Oracle
Server=10.64.60.51
Driver=dbapi.dll
ProtheusOnly=0
Port=7870

[Drivers]
Active=TCP

[TCP]
TYPE=TCPIP
Port=1952

[TDS]
AllowApplyPatch=*
AllowEdit=*

[General]
ConnectionTimeOut=720
InstallPath=C:\ERP_PROTHEUS\Homologacao
Segmento=UZHU=G62
Serie===AV
ServerMemoryLimit=1024
DebugThreadUsedMemory=0
ctreeMode=Server
;ConsoleLog=1
;ConsoleFile=C:\ERP_PROTHEUS\Homologacao\protheus_data\log\apo_wsjob.log
;BuildKillUsers=1


[CtreeServer]
ctServerName=FAIRCOMS@10.64.60.51

[Licenseserver]
Enable=0
Port=5554
Showstatus=0
Enablenumber=0

[LICENSECLIENT]
server=172.20.0.139
port=5557

[Service]
Name=05-P12_HOMOLOG_WSJob
DisplayName=05-P12_HOMOLOG_WSJob

[MAIL]
Protocol=IMAP

[ONSTART]
Jobs =JOB_WSUNIMED_0302, U_UNIJOB 
;Jobs = U_UNIJOB
;;Jobs =JOB_WSUNIMED_0302,U_WSREENVIAR, U_UNIJOB
;;,FWSCHDQUEUE_WS_JOB
Refreshrate =60

[U_UNIJOB]
Main = U_UNIJOB
Environment = APO_WSJOB

[http]
Enable=1
Path=C:\ERP_PROTHEUS\Homologacao\protheus_data\web     
Port=7080

[JOB_WSUNIMED_0302]
TYPE=WEBEX
ENVIRONMENT=APO_WSJOB
INSTANCES=1,5
SIGAWEB=WS
INSTANCENAME=WSUNIMED
ONSTART=__WSSTART
ONCONNECT=__WSCONNECT
PREPAREIN=03,02

[JOB_PORTAL]
TYPE=WEBEX
ENVIRONMENT=APO_WSJOB
INSTANCES=1,5
SIGAWEB=PORTAL
INSTANCENAME=portal
ONSTART=__WSSTART
ONCONNECT=__WSCONNECT
PREPAREIN=03,02
ONEXIT=FINISHWEBEX
WEBSERVICELOCATION=http://10.64.60.51:7080/

[10.64.60.51:7080]
ENABLE=1
PATH=C:\ERP_PROTHEUS\Homologacao\protheus_data\web\wsunimed
ENVIRONMENT=APO_WSJOB
INSTANCENAME=WSUNIMED
RESPONSEJOB=JOB_WSUNIMED_0302
DEFAULTPAGE=wsindex.apw

[10.64.60.51:7080/portal]
ENABLE=1
PATH=C:\ERP_PROTHEUS\Homologacao\protheus_data\web\portal
ENVIRONMENT=APO_WSJOB
INSTANCENAME=portal
RESPONSEJOB=JOB_PORTAL
DEFAULTPAGE=wsindex.apw

[U_WSREENVIAR]
Main=U_WSREENVIAR
ENVIRONMENT=APO_WSJOB

[FWSCHDMANAG_WS_JOB]
Main=FWSCHDMANAG
Environment=APO_WSJOB

[FWSCHDQUEUE_WS_JOB]
Main=FWSCHDQUEUE
Environment=APO_WSJOB

``````

**03 - Processos RPC ( Remote Process Call )**

Foram criadas rotinas e compiladas no ambiente "APO_WSJOB", essas rotinas são chamados pelo processo "UNI_JOB" e são executadas conforme definições da áreas de negocio. Para ver o programa fonte [click aqui](https://labs.unimedgoiania.coop.br/ti/setsis/desenvolvimento/protheus/protheus-unimed/blob/master/ProjetosRPC/ProjetoJOB/Fun%C3%A7%C3%B5es/UNIJOB.prw)