Nesta página vamos descrever os lançamentos padrões utilizados na geração de "documento de saída", o qual gera registros no modulo financeiro em contas a receber. Seus LPs foram também revisados.

Os lançamentos padrões revisados foram:

610-001 - Documento de saída ( Inclusão de lançamentos por Itens ), nesse LP está configurado para buscar a conta de crédito no cadastro dos produtos que compõe os itens da nota fiscal. ( Por esse LP, obtém o valor bruto faturado )

**620-001** - Documento de saída ( Inclusão de documento total ), nesse LP está configurado para buscar a conta contabil no cadastro da natureza financeira, no campo ED_DEBITO. ( Por este LP, obtém o valor líquido à receber ).

**620-002** - Documento de saída ( valor do imposto ISS )

**620-003** - Documento de saída ( valor do imposto IRRF )

**620-004** - Documento de saída ( valor das contribuições PIS/COFINS/CSLL )

**630-001** - Documento de saída ( Exclusão de lançamentos por Itens ). Idem ao 610-001, porém com as contas contábeis trocadas entre débito e credito.

**635-001** - Documento de saída ( Exclusão de documento total ). Idem ao 620-001, porém com as contas contábeis trocadas entre débito e crédito.

**635-002** - Exclusão documento de saída ( Valor do ISS ).

**635-003** - Exclusão documento de saída ( Valor do IRRF ).

**635-004** - Exclusão documento de saída ( Valor valor das contribuições PIS/COFINS/CSLL )





