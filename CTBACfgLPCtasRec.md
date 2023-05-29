Nesta página vamos descrever as regras definidas pelo setor de controladoria para a utilização dos lançamentos padrões  do ERP para geração de lançamentos contábeis do Contas à Receber.

**LP500 - Inclusão de título a receber** 
```
Código	Descrição
500-001	INCLUSAO CTAS RECEBER - TITULO NORMAL - VALOR BRUTO 
500-002	INCLUSAO CTAS RECEBER - TITULO NORMAL - VALOR LIQUIDO  
500-003	INCLUSAO CTAS RECEBER - TITULO NORMAL - IRRF  
500-004	INCLUSAO CTAS RECEBER - TITULO NORMAL - PIS     
500-005	INCLUSAO CTAS RECEBER - TITULO NORMAL - COFINS  
500-006	INCLUSAO CTAS RECEBER - TITULO NORMAL - CSLL  
500-007	INCLUSAO CTAS RECEBER - TITULO NORMAL - ISS     
500-008	INCLUSAO CTAS RECEBER - TITULO ABATIMENTO ("AB-") 
```
**LP505 - Exclusão de título a receber** 
```
Código	Descrição
505-001	EXCLUSAO CTAS RECEBER - TITULO NORMAL - VALOR BRUTO    
505-002	EXCLUSAO CTAS RECEBER - TITULO NORMAL - VALOR LIQUIDO 
505-003	EXCLUSAO CTAS RECEBER - TITULO NORMAL - IRRF  
505-004	EXCLUSAO CTAS RECEBER - TITULO NORMAL - PIS     
505-005	EXCLUSAO CTAS RECEBER - TITULO NORMAL - COFINS 
505-006	EXCLUSAO CTAS RECEBER - TITULO NORMAL - CSLL  
505-007	EXCLUSAO CTAS RECEBER - TITULO NORMAL - ISS 
505-008	EXCLUSAO CTAS RECEBER - TITULO ABATIMENTO ("AB-") 
```
**LP507 - Exclusão de título à receber**  
```
507-001	EXC TIT REC MUL NAT  
```

**LP520 - Baixa de titulo à receber ( "0" - Em carteira )** 
```
Código	Descrição
520-001	BAIXA CTAS REC - SITUACAO "0" CARTEIRA - BANCO/CAIXA   
520-002	BAIXA CTAS REC - SITUACAO "0" CARTEIRA - DESCONTO CONCEDIDO 
520-003	BAIXA CTAS REC - SITUACAO "0" CARTEIRA - JUROS RECEBIDO  
520-004	BAIXA CTAS REC - SITUACAO "0" CARTEIRA - MULTA RECEBIDA 
520-005	BAIXA CTAS REC - SITUACAO "0" CARTEIRA - VALOR NOMINAL  
```
**LP521 - Baixa de título à receber ( "1" - Em cobrança simples )**
```
Código	Descrição
521-001	BAIXA CTAS REC - SITUACAO "1" COB SIMPLES - BANCO/CAIXA        
521-002	BAIXA CTAS REC - SITUACAO "1" COB SIMPLES - DESCONTO CONCEDIDO 
521-003	BAIXA CTAS REC - SITUACAO "1" COB SIMPLES - JUROS RECEBIDO   
521-004	BAIXA CTAS REC - SITUACAO "1" COB SIMPLES - MULTA RECEBIDA  
521-005	BAIXA CTAS REC - SITUACAO "1" COB SIMPLES - VALOR NOMINAL    
```
**LP527 - Estorno de baixa título à receber**
```
Código	Descrição
527-001	ESTORNO BAIXA CTAS REC - SITUACAO "0/1" CARTEIRA/COB SIMPLES - BANCO/CAIXA   
527-002	ESTORNO BAIXA CTAS REC - SITUACAO "0/1" CARTEIRA/COB SIMPLES - DESCONTO CONCEDIDO 
527-003	ESTORNO BAIXA CTAS REC - SITUACAO "0/1" CARTEIRA/COB SIMPLES - JUROS RECEBIDO  
527-004	ESTORNO BAIXA CTAS REC - SITUACAO "0/1" CARTEIRA/COB SIMPLES - MULTA RECEBIDA  
527-005	ESTORNO BAIXA CTAS REC - SITUACAO "0/1" CARTEIRA/COB SIMPLES - VALOR NOMINAL    
```
**LP610 - Geração doc saída ( produto )**
```
Código	Descrição
610-001	MEDICINA OCUPACIONAL(RECEITA C/SERVICOS)  
```
**LP620 - Geração doc saída**
```
Código	Descrição
620-001	MEDICINA OCUPACIONAL (CLIENTES) 
620-002	MEDICINA OCUPACIONAL (ISS)      
620-003	MEDICINA OCUPACIONAL (IRRF)         
620-004	MEDICINA OCUPACIONAL (PIS/COF/CSLL/IRPJ)  
```
**LP630 - Exclusão doc saída ( produto )**
```
Código	Descrição
630-001	MEDICINA OCUP (EXCL. RECEITA C/SERVICOS) 
```

**LP635 - Exclusão doc saída**
```
Código	Descrição
635-001	MEDICINA OCUP (EXCL. CLIENTES)  
635-002	MEDICINA OCUP (EXCL. ISS)      
635-003	MEDICINA OCUP (EXCL. IRRF)    
635-004	MEDICINA OCUP (EXCL. PIS/COF/CSLL/IRPJ) 
```

**Script de ajustes LPs 520,521 e 527**
```
declare
cursor c1 is
  select ct5.ct5_lanpad
       , ct5.ct5_sequen
       , ct5.ct5_debito
       , ct5.ct5_credit
  from   siga.ct5030                       ct5
  where  ct5.d_e_l_e_t_                    = ' '
  and    ct5.ct5_filial                    = '01'
  and (( ct5.ct5_lanpad                    = '520'  and  ct5.ct5_sequen = '001' ) or
       ( ct5.ct5_lanpad                    = '521'  and  ct5.ct5_sequen = '001' ) or
       ( ct5.ct5_lanpad                    = '527'  and  ct5.ct5_sequen = '001' ) )
  ;     
begin
  for r1 in c1 loop
    if r1.ct5_lanpad = '520' and r1.ct5_sequen = '001' then
       update  siga.ct5030     ct5 set
          ct5.ct5_debito = r1.ct5_debito
       where ct5.ct5_filial    != '01'   
       and   ct5.ct5_lanpad     = r1.ct5_lanpad
       and   ct5.ct5_sequen     = r1.ct5_sequen;       
    end if;  
    -------
    commit;
    -------
    if r1.ct5_lanpad = '521' and r1.ct5_sequen = '001' then
       update  siga.ct5030     ct5 set
          ct5.ct5_debito = r1.ct5_debito
       where ct5.ct5_filial    != '01'   
       and   ct5.ct5_lanpad     = r1.ct5_lanpad
       and   ct5.ct5_sequen     = r1.ct5_sequen;       
    end if;  
    -------
    commit;
    -------
    if r1.ct5_lanpad = '527' and r1.ct5_sequen = '001' then
       update  siga.ct5030     ct5 set
          ct5.ct5_credit = r1.ct5_credit
       where ct5.ct5_filial    != '01'   
       and   ct5.ct5_lanpad     = r1.ct5_lanpad
       and   ct5.ct5_sequen     = r1.ct5_sequen;       
    end if;  
    ------
    commit;
    ------    
  end loop;
end;  
```