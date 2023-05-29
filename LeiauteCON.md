Nesse documento vamos descrever a sequência das execuções das tarefas para que ocorra a integração do SGU 2.0 para o ERP ( Protheus ).

Nesse leiaute são enviados os dados dos lançamentos contábeis gerados no SGU 2.0 para serem integrados com o ERP. São lançamentos de vários tipos de lotes contábeis conforme manual do sistema.

Manual do SGU 2.0 : https://confluence.fesctecnologia.com.br/pages/viewpage.action?pageId=19023231

**Query utilizada para obter o leiaute**
``````
SELECT  PR.*
      , PS.COMAND_SQL
      , CURSOR ( SELECT *
                   FROM  DBAUNIMED.PTU_LAYOUT_DET PLD
                  WHERE  PLD.COD_ARQ = PS.COD_ARQ
                    AND  PLD.COD_REG = PS.COD_REG
                  ORDER  BY 1,2,3 )                             LAYOUT
FROM   PTU_LAYOUT_SQL PS
      ,PTU_LAYOUT_REG PR     
WHERE  PR.COD_ARQ = PS.COD_ARQ
AND    PR.COD_REG = PS.COD_REG
--> 91 (ENVIO SGU X ERP) 92 (RETORNO ERP X SGU)
AND    PS.COD_ARQ = 91  
--> LEIAUTE DO LANCTO CONTABIL
AND    PR.SIG_REG LIKE '%INTGR_CON%'
ORDER  BY 1, 2
``````

Nesse documento fizemos a classificação dos tipos de lotes do **FATURAMENTO** e **CUSTOS**

**Lotes FATURAMENTO**

|LOTE SGU| LOTE ERP  |DESCRIÇÃO					        |ATIVO  |INFORMAÇÕES SOBRE O LOTE																																	   |
|--------|-----------|--------------------------------------------------|-------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|FAT     |020000     |Faturamento - Faturas emitidas			|Sim	|Contabilização da receita, contabiliza todas as guias conforme sua receita, considerados os tipos																								   |
|AFA     |022000     |Faturamento - A faturar				|Sim	|Contabilização das guias não faturadas dentro da sua competência, competência da guia < competência do faturamento ou sem fatura 																				   |
|FCC     |021000     |Faturamento - Faturas canceladas			|Sim	|Contabilização das faturas canceladas, abertura conforme ANS.  Faturas emitidas contra titular e/ou empresa (ambos clientes local)																				   |
|FIN     |031000     |Faturamento - Intercâmbio				|Sim	|Contabilização das faturas de intercâmbio, abertura conforme ANS. (Faturas emitidas contra Unimeds)																								   |
|FIC     |032000     |Faturamento - Intercâmbio - Faturas canceladas	|Sim	|Contabilização das faturas de intercâmbio canceladas, abertura conforme ANS (Faturas contra Unimeds)																								   |
|AIR     |035000     |AJIUS - Intercâmbio a Receber / Glosas Recebidas 	|Sim	|Contabilização das faturas de ajuste no AJUIUS a receber																													   |
|CCP     |018000     |Conta Corrente Prestador				|Sim	|Contabilização das faturas emitidas contra o médicos que ficaram negativos no conta corrente ou MDEB																								   |
|PRT     |023000     |Pro Rata (PPCNG)					|Sim	|Contabilização do Pro Rata (PPCNG): contabilização da receita (mensalidades), abrindo conforme o rateio de atos (Principal, Auxiliar e Não Cooperado) 																		   |



**Lotes CUSTO**

|LOTE SGU| LOTE ERP  |DESCRIÇÃO					        |ATIVO  |INFORMAÇÕES SOBRE O LOTE																																	   |
|--------|-----------|--------------------------------------------------|-------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|CUS	 |010000     |Custos com Atendimento Local			|Sim	|Contabilização do custo local (beneficiários internos e externos atendidos dentro da rede de atendimento, conhecimento da IN32, Reversão do IN32. Considerando os tipos de registros:														   |
|CEV	 |012000     |Custo com Eventos de Pagamento sem Guias		|Sim	|Contabilização do custo com lançamentos manuais onde foram utilizados eventos.																											   |
|CIN	 |011000     |Custos com Intercâmbio				|Sim	|Contabilização do custo de Intercâmbio, beneficiários nossos atendidos na rede externa. Considerando os tipos de registro																					   |
|NICUS	 |Não usa    |NI - Custo					|Sim	|Lote do novo intercâmbio, contabilização do custo. Mesmo conceito do lote CUS, porém para guias de origem do novo intercâmbio. 																				   |
|CRE	 |015000     |Custos com Repasse				|Sim	|																																				   |
|ITCAP	 |013000     |Integralização de Capital 			|Sim	|Contabilização dos descontos realizados na produção médica do cooperado referente a integralização do capital social																						   |
|GNP	 |016000     |Guias não Processadas 				|Sim	|Contabilização das guias não processadas, considera as guias que não forma processadas na competência de contabilização, neste caso é considerada a data de entrada da guia e o seu status como não processada											   |
|RNP	 |017000     |Reversão das Guias não Processadas		|Sim	|Reversão das guias não processadas, considera as guias que foram processadas na competência posterior a da sua entrada. Neste caso é feito um lançamento inverso do lote contabilizado anteriormente do GNP. Será gerado somente se a guia foi contabilizada anteriormente em um lote GNP.	   |
|PGR	 |040000     |Pagamento a Rede					|Sim	|Contabilização do pagamento a Rede, considerando os eventos para contabilização																										   |
|NOP	 |041000     |Nota de Pagamento					|Sim	|"Contabilização das Notas de Pagamento, sendo elas:( Reembolso; OPME; Faturas de intercambio a pagar (vencimento);Remoção;  Notas de débitos;)																			   |
|NIGNP	 |Não usa    |NI - Guias não Processadas			|Sim	|Lote do novo intercâmbio, Contabilização das Guias não Processadas do novo intercâmbio, mesmo conceito do GNP, porém para guias de origem do novo intercâmbio																	   |
|NIRNP	 |Não usa    |NI - Reversão das Guias não Processadas		|Sim	|Lote do novo intercâmbio, Contabilização da reversão das guias não processadas. Mesmo conceito do lote RNP, porém para guias de origem do novo intercâmbio																	   |
|NIPGR	 |Não usa    |NI - Pagamento ao Prestador			|Sim	|Lote do novo intercâmbio, contabilização do Pagamento a Rede, Mesmo conceito do lote PGR, porém para pagamentos do novo intercâmbio. 																				   |
|NITMD	 |Não usa    |NI - Despesa com Taxa e Margem Econômica		|Sim	|Lote do novo intercâmbio, contabilização das Despesas com Taxa e Margem Econômica do novo intercâmbio.																								   |
|NITMR	 |Não usa    |NI - Receita com Taxa e Margem Econômica		|Sim	|Lote do novo intercâmbio, contabilização das Receitas com Taxa e Margem Econômica do novo intercâmbio																								   |
|NDU	 |033000     |Notas de Débito contra Unimeds			|Sim	|																																				   |
|INTPR	 |Não usa    |Atendimento Interprestadora			|Sim	|																																				   |
|AIP	 |034000     |AJIUS - Intercâmbio a Pagar			|Sim	|Contabilização das faturas de ajuste no AJUIUS a pagar																														   |
|RSUS	 |014000     |Ressarcimento ao SUS				|Sim	|Lote referente aos atendimentos importados do sistema SGU-RESSUS																												   |
|PGX	 |050000     |Baixa do Pagamento a Rede				|Sim	|Lote referente a contabilização das baixas do pagamento a rede.																												   |

Obs. No dia 28/08/2019 modifiquei a query do SGU utilizada para enviar os lançamentos contábeis para fila de integração com ERP. A alteração foi feita na view "v_intgra_padr_con", acrescentei o "order by  order by lctbl.lctbl_dat". Será apenas para testes no ambiente de homologação; o que se espera com essa modificação é geração dos lançamentos contábeis na integração com ERP por ordem de data.

Código modificado
``````
create or replace view v_intgra_padr_con as
select /* Versão: 000 */
       lctbl.rowid cod_rowid,
       lctb.lctb_tip_ctabil lancamento_padrao,
       lctbl.lctbl_tip tipo_lancamento,
       lctb.lctb_nro_compet competencia_contabil,
       lctbl.lctbl_dat data_lancamento,
       cast(null as varchar2(1)) reservado1,
       lctbl.lctbl_cod_conta_contab_debito conta_debito,
       cast(null as varchar2(1)) reservado2,
       lctbl.lctbl_cod_conta_contab_cred conta_credito,
       lctbl.lctbl_cod_unimed_debito unimed_debito,
       lctbl.lctbl_cod_unimed_cred unimed_credito,
       lctbl.lctbl_val * 100 valor,
       lctbl.lctbl_des_his historico,
       lctbl.lctbl_cod_centro_custo_debito cc_debito,
       lctbl.lctbl_cod_centro_custo_cred cc_credito,
       f_busca_val_dmdp('DMDP_IND_UNIMED_RESP_FILIAL_ERP',
                        f_conv_num(f_busca_param('FX_COD_UNIMED_SISTEM'))) filial,
       lctb.lctb_nro_lote lote_contabil,
       lctbl.lctbl_nro_seq sequencial,
       decode(k_geral.f_verif_null(lctb.lctb_dat_solic_cancel), null, 'E', 'C') situacao,
       k_geral.f_verif_null(lctb.lctb_dat_solic_cancel) data_cancel,
       k_geral.f_verif_null(lctb.lctb_des_solic_motivo_cancel) des_motivo_cancel,
       lctbl.lctbl_cod_item_contab_debito item_debito,
       lctbl.lctbl_cod_item_contab_cred item_credito
  from lote_ctabil lctb, lote_ctabil_lacto lctbl
 where lctb.lctb_nro_lote = lctbl.lctb_nro_lote
** order by lctbl.lctbl_dat**
 ;
``````
