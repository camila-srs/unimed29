1 - Controle de Código Fonte (Ícone do menu lateral esquerdo);

2 - Modo de Exibições e Mais Ações;

3 - Efetuar _Pull_;

4 - Validar o que foi inserido novo:

	4.1 - Documentos;
	4.2 - projetoerpunimed;
	4.3 - Controle_Atualizações>2022>MES;
	4.4 - Acessar e-mail de _comit_, ver trecho alterado e conferir na lupa conforme o nome do fonte *.PRW.

Preparando para a atualização do próximo dia:

1 - Fazer o _Backup_ do RPO atual. com a nomenclatura *AAAAMMDD.RPO;

Diretório: \\\TOTVS04-VM\e$\Totvs\ProtheusV12\repositorios\custom

2 - Verificar o status do serviço PROVISORIO2 e realizar preparação do RPO para receber a atualização:

	Se serviço on: O mesmo deve ser parado para que o prov2.rpo seja substituído pelo o RPO unimed.rpo disponível no endereço:
	\\TOTVS04-VM\e$\Totvs\ProtheusV12\repositorios\custom e renomeado para prov2.rpo

	Se serviço of: O prov2.rpo deve ser substituir pelo o RPO unimed.rpo disponível no endereço:
	\\TOTVS04-VM\e$\Totvs\ProtheusV12\repositorios\custom e renomeado para prov2.rpo

	Após a substituição do RPO o serviço PROVISORIO2 deve ser reestabelecido.

3 - Compilar os fontes do arquivo de atualização - VSCode:

	3.1 - Conectar no provisorio2 Produção;
	3.2 - Compilar os fontes (CTRL+F9);
	3.3 - Desfragmentar o RPO;
	3.4 - Desconectar do ambiente provisorio2;
	3.5 - Para serviço do PROVISORIO2.

4 - Para o ambiente de produção, seguir os seguintes passos:

	1 - Aplicação;
	2 - DBACESS;
	3 - Licence Sever;
	4 - Ctree.

	Ordem para parar os serviços (1,2,3,4);

5 - Substituir os RPOS;

6 - Efetuar limpeza das pastas, devendo fazer um _Backup_ dos arquivos a serem excluídos:

	6.1 - No diretório e:\Totvs\ProtheusV12\protheus_data\System
	6.2 - Filtrar pelos seguintes nomes:
		*.CDX, *.IDX, *.IND, *.LOG, TOTVSSP*.*
	6.3 - No diretório e:\Totvs\ProtheusV12\protheus_data\System\ctreeint
		Deletar todos os arquivos.
	6.4 - No diretório e:\Totvs\ProtheusV12\protheus_data\ctreeint
		Deletar todos os arquivos.
	6.5 - No diretório e:\Totvs\ProtheusV12\protheus_data\spool
		Deletar todos os arquivos.
	6.6 - No diretório e:\Totvs\ProtheusV12\protheus_data\generico
		Deletar todos os arquivos.
	6.7 - No diretório e:\Totvs\C-tree\FairCom\V9.5.2\winX64\bin\ace\isam\data
		Deletar os arquivos *.FCS.

7 - Subir os serviços em modo exclusivo:

	1 - Ctree;
	2 - Licence Sever;
	3 - DBACESS;
	4 - Aplicação.

	Ordem para subir novamente (1,2(Aguardar 10S),3 (Conferir usuários), 4 (Subir apenas o provisorio2)).

8 - Acessar o ambiente provisorio2 no SIGACFG e logar nas empresas: 03,04 e 71;

9 - Preparar ambiente para produção:

	9.1 Para o ambiente de produção, seguir os seguintes passos:
	1 - Aplicação;
	2 - DBACESS;
	3 - Licence Sever;
	4 - Ctree.

	Ordem para parar os serviços (1,2,3,4);

	9.2 Subir os serviços:
	1 - Ctree;
	2 - Licence Sever;
	3 - DBACESS;
	4 - Aplicação;
	5 - Teste Aplicação;

	Ordem para subir novamente (1, 2 (Aguardar 10S), 3 (Conferir usuários), 4).

10 - Enviar e-mail de notificação de conclusão da atualização; 

Apêndice - Modelos de e-mail referente a atualização semanal:

[PARALISACAO_PROTHEUS_ENVIO.msg](uploads/20f6094ec504bf387c36cde1931f6955/PARALISACAO_PROTHEUS_ENVIO.msg)

[PARALISACAO_PROTHEUS_RESTAURA.msg](uploads/75525d89d21a0db88bd719943f829835/PARALISACAO_PROTHEUS_RESTAURA.msg)

[PARALISAÇÃO_DO_PROTHEUS-CANCELA.msg](uploads/d8778383976baff91b56c696295e0979/PARALISAÇÃO_DO_PROTHEUS-CANCELA.msg)

11 - Preencher a planilha de atualização no diretório abaixo.

W:\GERTI$\TI_Comum\COMUM\Estatísticas - TI

***By Cristiano Ferreira e Samuel Rodrigues
20-06-2022**