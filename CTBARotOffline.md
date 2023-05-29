Nessa página vamos tratar da rotina customizada da qual faz a chamada da rotina original de contabilização offline ( FINA370).

Foi criada a rotina UACTB013 a qual faz a chamado da rotina CTBAFIN (padrão TOTVS), mas antes faz o preenchimento dos parâmetros exigidos pela rotina padrão.

Esse rotina customizada será executado através do JOB de integração, o tempo de execução será uma vez no dia. Ou seja,  vai observar os movimentações realizadas no ERP e na madrugada do dia seguinte fará a geração dos lançamentos e pré-lançamentos.

A cada execução ficará registrado no arquivo "console.log" do appserver do job de integração. Conforme exemplo abaixo.

```
----------------------------->>> INTEGRACAO SGU 2.0 <<<----------------------------------------
Data  -> : 07/10/19 - Hora : 19 Minuto : 35 Segundo : 38
-----------------------------------------------------------------------------------------------
 07/10/19 19:35:48 Rotina UAFIN023 - Job de Importacao de FORNECEDORES
 07/10/19 19:35:48 Rotina UAFIN024 - Job de Importacao de CONTAS A PAGAR
 07/10/19 19:35:48 Rotina UAFIN020 - Job de Importacao de CLIENTES
 07/10/19 19:36:16 Rotina UACTB013 - Job de Lctos CTB Off-line !
 07/10/19 19:36:16 Rotina UACTB013 - Gerando CTB Offline "CTAS REC" da filial: 01 - Adm                                     
 07/10/19 19:36:37 Rotina UACTB013 - Gerando CTB Offline "CTAS PAG" da filial: 01 - Adm                                     
 07/10/19 19:36:43 Rotina UACTB013 - Gerando CTB Offline "CHEQUES"  da filial: 01 - Adm                                     
 07/10/19 19:36:47 Rotina UACTB013 - Gerando CTB Offline "CTAS REC" da filial: 02 - SAU1                                    
 07/10/19 19:36:51 Rotina UACTB013 - Gerando CTB Offline "CTAS PAG" da filial: 02 - SAU1                                    
 07/10/19 19:36:55 Rotina UACTB013 - Gerando CTB Offline "CHEQUES"  da filial: 02 - SAU1                                    
 07/10/19 19:36:59 Rotina UACTB013 - Gerando CTB Offline "CTAS REC" da filial: 03 - Cto Clinico                             
 07/10/19 19:37:03 Rotina UACTB013 - Gerando CTB Offline "CTAS PAG" da filial: 03 - Cto Clinico                             
 07/10/19 19:37:07 Rotina UACTB013 - Gerando CTB Offline "CHEQUES"  da filial: 03 - Cto Clinico                             
 07/10/19 19:37:11 Rotina UACTB013 - Gerando CTB Offline "CTAS REC" da filial: 04 - Flamboyant                              
 07/10/19 19:37:15 Rotina UACTB013 - Gerando CTB Offline "CTAS PAG" da filial: 04 - Flamboyant                              
 07/10/19 19:37:20 Rotina UACTB013 - Gerando CTB Offline "CHEQUES"  da filial: 04 - Flamboyant                              
 07/10/19 19:37:24 Rotina UACTB013 - Gerando CTB Offline "CTAS REC" da filial: 05 - SAU2                                    
 07/10/19 19:37:28 Rotina UACTB013 - Gerando CTB Offline "CTAS PAG" da filial: 05 - SAU2                                    
 07/10/19 19:37:32 Rotina UACTB013 - Gerando CTB Offline "CHEQUES"  da filial: 05 - SAU2                                    
 07/10/19 19:37:36 Rotina UACTB013 - Gerando CTB Offline "CTAS REC" da filial: 06 - Call Center                             
 07/10/19 19:37:40 Rotina UACTB013 - Gerando CTB Offline "CTAS PAG" da filial: 06 - Call Center                             
 07/10/19 19:37:45 Rotina UACTB013 - Gerando CTB Offline "CHEQUES"  da filial: 06 - Call Center                             
 07/10/19 19:37:49 Rotina UACTB013 - Gerando CTB Offline "CTAS REC" da filial: 07 - Lab Rio Verde                           
 07/10/19 19:37:53 Rotina UACTB013 - Gerando CTB Offline "CTAS PAG" da filial: 07 - Lab Rio Verde                           
 07/10/19 19:37:57 Rotina UACTB013 - Gerando CTB Offline "CHEQUES"  da filial: 07 - Lab Rio Verde                           
 07/10/19 19:38:01 Rotina UACTB013 - Gerando CTB Offline "CTAS REC" da filial: 08 - Quimioterapia                           
 07/10/19 19:38:05 Rotina UACTB013 - Gerando CTB Offline "CTAS PAG" da filial: 08 - Quimioterapia                           
 07/10/19 19:38:09 Rotina UACTB013 - Gerando CTB Offline "CHEQUES"  da filial: 08 - Quimioterapia                           
 07/10/19 19:38:13 Rotina UACTB013 - Gerando CTB Offline "CTAS REC" da filial: 09 - Clin Psicologia                         
 07/10/19 19:38:17 Rotina UACTB013 - Gerando CTB Offline "CTAS PAG" da filial: 09 - Clin Psicologia                         
 07/10/19 19:38:22 Rotina UACTB013 - Gerando CTB Offline "CHEQUES"  da filial: 09 - Clin Psicologia                         
 07/10/19 19:38:26 Rotina UACTB013 - Gerando CTB Offline "CTAS REC" da filial: 10 - Setpass                                 
 07/10/19 19:38:30 Rotina UACTB013 - Gerando CTB Offline "CTAS PAG" da filial: 10 - Setpass                                 
 07/10/19 19:38:34 Rotina UACTB013 - Gerando CTB Offline "CHEQUES"  da filial: 10 - Setpass                                 
 07/10/19 19:38:38 Rotina UACTB013 - Gerando CTB Offline "CTAS REC" da filial: 11 - Nucleo de Atencao Integral a Saude(NAIS)
 07/10/19 19:38:42 Rotina UACTB013 - Gerando CTB Offline "CTAS PAG" da filial: 11 - Nucleo de Atencao Integral a Saude(NAIS)
 07/10/19 19:38:46 Rotina UACTB013 - Gerando CTB Offline "CHEQUES"  da filial: 11 - Nucleo de Atencao Integral a Saude(NAIS)
 07/10/19 19:38:50 Rotina UACTB013 - Gerando CTB Offline "CTAS REC" da filial: 12 - Jardim America                          
 07/10/19 19:38:54 Rotina UACTB013 - Gerando CTB Offline "CTAS PAG" da filial: 12 - Jardim America                          
 07/10/19 19:38:58 Rotina UACTB013 - Gerando CTB Offline "CHEQUES"  da filial: 12 - Jardim America                          
 07/10/19 19:39:02 Rotina UACTB013 - Gerando CTB Offline "CTAS REC" da filial: 13 - Central Administrativa                  
 07/10/19 19:39:06 Rotina UACTB013 - Gerando CTB Offline "CTAS PAG" da filial: 13 - Central Administrativa                  
 07/10/19 19:39:10 Rotina UACTB013 - Gerando CTB Offline "CHEQUES"  da filial: 13 - Central Administrativa                  
 07/10/19 19:39:14 Rotina UACTB013 - Gerando CTB Offline "CTAS REC" da filial: 14 - Centro de Diagnosticos Unimed           
 07/10/19 19:39:18 Rotina UACTB013 - Gerando CTB Offline "CTAS PAG" da filial: 14 - Centro de Diagnosticos Unimed           
 07/10/19 19:39:22 Rotina UACTB013 - Gerando CTB Offline "CHEQUES"  da filial: 14 - Centro de Diagnosticos Unimed           
 07/10/19 19:39:27 Rotina UACTB013 - Gerando CTB Offline "CTAS REC" da filial: 15 - Espaco Sinta-se Bem                     
 07/10/19 19:39:31 Rotina UACTB013 - Gerando CTB Offline "CTAS PAG" da filial: 15 - Espaco Sinta-se Bem                     
 07/10/19 19:39:35 Rotina UACTB013 - Gerando CTB Offline "CHEQUES"  da filial: 15 - Espaco Sinta-se Bem                     
 07/10/19 19:39:40 Rotina UACTB013 - Gerando CTB Offline "CTAS REC" da filial: 16 - Posto de Coleta Sao Francisco           
 07/10/19 19:39:44 Rotina UACTB013 - Gerando CTB Offline "CTAS PAG" da filial: 16 - Posto de Coleta Sao Francisco           
 07/10/19 19:39:48 Rotina UACTB013 - Gerando CTB Offline "CHEQUES"  da filial: 16 - Posto de Coleta Sao Francisco           
 07/10/19 19:39:52 Rotina UACTB013 - Gerando CTB Offline "CTAS REC" da filial: 17 - Posto de Coleta CASAG                   
 07/10/19 19:39:56 Rotina UACTB013 - Gerando CTB Offline "CTAS PAG" da filial: 17 - Posto de Coleta CASAG                   
 07/10/19 19:40:00 Rotina UACTB013 - Gerando CTB Offline "CHEQUES"  da filial: 17 - Posto de Coleta CASAG                   
 07/10/19 19:40:04 Rotina UACTB013 - Gerando CTB Offline "CTAS REC" da filial: 18 - Unidade Coleta Marista                  
 07/10/19 19:40:08 Rotina UACTB013 - Gerando CTB Offline "CTAS PAG" da filial: 18 - Unidade Coleta Marista                  
 07/10/19 19:40:12 Rotina UACTB013 - Gerando CTB Offline "CHEQUES"  da filial: 18 - Unidade Coleta Marista                  
 07/10/19 19:40:16 Rotina UACTB013 - Gerando CTB Offline "CTAS REC" da filial: 19 - Unimed Crianca                          
 07/10/19 19:40:20 Rotina UACTB013 - Gerando CTB Offline "CTAS PAG" da filial: 19 - Unimed Crianca                          
 07/10/19 19:40:24 Rotina UACTB013 - Gerando CTB Offline "CHEQUES"  da filial: 19 - Unimed Crianca                          

```