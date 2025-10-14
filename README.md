# BD-II
Aulas de BD-II (Saldanha)

tabela no from (quero todos) 
a segunda tabela (left join)

*crude
-create = insert
-read = selet
-update = update
-delete = delete

where / order / truncate / droptable

New = recebe o novo (insert e update)
old = recebe o antigo (insert e update)
TG_NAME (nome da trigger)
TG_WHEN ( momento da execução 'BEFORE' ou 'AFTER'
TG_LEVEL (nivel do  trigger 'ROW' ou 'STATEMENT'
TG_OP (operação que disparou 'INSERT' , 'UPDATE', 'DELETE' ou 'TRUNCADE'
TG_RELID (OID da tabela que disparou o trigger)
TG_TABLE_NAME (nome da tabela)
TG_TABLE_SCHEMA ( nome do schema onde esta a tabela)
TG_ARGV [] text [] (array com argumentos passados para o trigger na criação (CREATE TRIGGER... EXECUTE FUNCTION ... (arg1, arg2,...)

evento = insert, delete, update...

--rollback VOLTA ATUALIZACAO
--begin ABRE TRANSACAO
--commit ATUALIZA ELA NO BANCO

-- Com - Rota GRADE Novo - GRADE H
SELECT FORMAT(DT_VALIDADE_OFERTA, 'dd/MM/yyyy') AS ROTA_VALIDADE,
       INCOTERM.SG_INCOTERM AS ROTA_INCOTERMS_SIGLA,
       mMerc.SG_MOEDA +' '+ FORMAT(rota.VL_INVOICE, '###,###,##0.00') AS valor_mercadoria,
       DS_FREQUENCIA AS ROTA_FREQUENCIA_DESCRICAO,
       CAST(dbo.OCSTransitTimeHistorico(Armador.CD_PESSOA, CD_ORIGEM, CD_DESTINO) AS nvarchar) AS VL_TRANSIT_TIME_HISTORICO,

  (SELECT STRING_AGG(tc.DS_COMMODITY, ', ')
   FROM OCS_EQUIPAMENTO_VOLUME ev WITH (NOLOCK)
   LEFT JOIN TAB_COMMODITY tc WITH (NOLOCK)ON tc.CD_COMMODITY = ev.CD_COMMODITY
   LEFT JOIN OCS_EQUIPAMENTO eq WITH (NOLOCK)ON eq.CD_MOVIMENTO_EQUIPAMENTO = ev.CD_MOVIMENTO_EQUIPAMENTO
   WHERE eq.CD_MOVIMENTO = rota.CD_MOVIMENTO) AS DS_MERCADORIA,
       ORIGEME.NM_LOCALIDADE AS ROTA_ORIGEM_EMBARQUE,
       DESTINO.NM_LOCALIDADE AS ROTA_DESTINO,
       'ROTA - ' + ISNULL(ORIGEM.NM_LOCALIDADE, 'XXX')+' X '+ ISNULL(DESTINO.NM_LOCALIDADE, 'XXX')+' X '+ ISNULL(DESTINOF.NM_LOCALIDADE, 'XXX')+ ' - '+ ISNULL(INCOTERM.NM_INCOTERM, 'XXX') + ' ( '+ ISNULL(ARMADOR.NM_FANTASIA, 'XXX') + ' )' AS ROTA_DESCRICAO,
       cast(VL_TRANSIT_TIME_DE AS varchar(4)) + isnull(' à ' + cast(VL_TRANSIT_TIME_ATE AS varchar(4)), '') + ' dias' AS ROTA_TRANSIT_TIME_VALOR,
       cast(VL_TRANSIT_TIME_COLETA_DE AS varchar(4)) + isnull(' à ' + cast(VL_TRANSIT_TIME_COLETA_ATE AS varchar(4)), '') + ' dias' AS TRANSIT_TIME_TEM_COLETA,
       CASE
           WHEN rota.ID_MOSTRAR_AGENTE = 1 THEN AGENTE.NM_FANTASIA
           ELSE NULL
       END AS AGENTE_FANTASIA,
       AGENTE.NM_PESSOA AS AGENTE_RAZAO_SOCIAL,
       AGENTE.DS_ENDERECO AS AGENTE_ENDERECO,
       AGENTE.NR_NUMERO AS AGENTE_NUMERO,
       AGENTE.NM_BAIRRO AS AGENTE_BAIRRO,
       PAIS_AGENTE.NM_PAIS_INGLES AS AGENTE_PAIS_INGLES,
       PAIS_AGENTE.NM_PAIS AS AGENTE_PAIS,
       ESTADO_AGENTE.SG_ESTADO AS AGENTE_ESTADO,
       AGENTE_LOCALIDADE.NM_LOCALIDADE AS AGENTE_CIDADE,
       AGENTE.DS_CEP AS AGENTE_ZIP_CODE,
       AGENTE.DS_FONE AS AGENTE_FONE,
       PAIS_ORIGEM.NM_PAIS AS ROTA_PAIS_ORIGEM,
       PAIS_ORIGEM.NM_PAIS_INGLES AS ROTA_PAIS_ORIGEM_INGLES,
       ORIGEM.NM_LOCALIDADE AS ROTA_ORIGEM ,
       destino.nm_localidade AS destino ,
  (SELECT STRING_AGG(d.NM_LOCALIDADE, '/')
   FROM OCS_TRECHO tr WITH (NOLOCK)
   LEFT JOIN LOC_LOCALIDADE d WITH (NOLOCK)ON d.CD_LOCALIDADE = tr.CD_LOCALIDADE
   WHERE tr.CD_MOVIMENTO = rota.CD_MOVIMENTO ) + CASE
                                                     WHEN destino.NM_LOCALIDADE = destinoF.NM_LOCALIDADE THEN '/' + isnull(destino.NM_LOCALIDADE, '')
                                                     ELSE ''
                                                 END AS transbordos,

  (SELECT STRING_AGG(d.NM_LOCALIDADE, '/')
   FROM OCS_TRECHO tr WITH (NOLOCK)
   LEFT JOIN LOC_LOCALIDADE d WITH (NOLOCK)ON d.CD_LOCALIDADE = tr.CD_LOCALIDADE
   WHERE tr.CD_MOVIMENTO = rota.CD_MOVIMENTO )AS rota_rental,
                                                 CASE
                                                     WHEN ID_MOSTRAR_ARMADOR = 1 THEN Armador.NM_FANTASIA
                                                     ELSE NULL
                                                 END AS rota_armador,
                                                 DESTINODTA.NM_LOCALIDADE AS ROTA_DESTINO_DTA,
                                                 DESTINOF.NM_LOCALIDADE AS ROTA_DESTINO_FINAL,
                                                 ENTREGA.NM_LOCALIDADE + ' - ' + estado.SG_ESTADO AS ROTA_CIDADE_ENTREGA,
                                                 ROTA.DS_ZIP_CODE_DESTINO AS ROTA_DESTINO_ZIP_CODE,
                                                 ROTA.DS_ZIP_CODE_ORIGEM AS ROTA_COLETA_ZIP_CODE,
                                                 PAIS_ENTREGA.NM_PAIS AS ROTA_PAIS_ENTREGA,
                                                 PAIS_ENTREGA.NM_PAIS_INGLES AS ROTA_PAIS_ENTREGA_INGLES,
                                                 PAIS_COLETA.NM_PAIS AS ROTA_PAIS_COLETA,
                                                 PAIS_COLETA.NM_PAIS_INGLES AS ROTA_PAIS_COLETA_INGLES,
                                                 COLETA.NM_LOCALIDADE AS ROTA_CIDADE_COLETA,
                                                 NR_TRAFEGO AS rota_trafego_numero,
                                                 DS_COMENTARIO_CLIENTE AS DS_OBSERVACAO_ROTA,
                                                 CASE
                                                     WHEN ocs_oferta.CD_PRODUTO in (2,
                                                                                    3,
                                                                                    5,
                                                                                    6) THEN
                                                            (SELECT string_agg(qt+'x'+ nm_equipamento, '/') AS ds_equipamento
                                                             FROM
                                                               (SELECT cast(sum(eq.QT_CONTAINER) AS varchar(3)) AS qt,
                                                                       tab_e.NM_EQUIPAMENTO
                                                                FROM OCS_MOVIMENTO ocs WITH (NOLOCK)
                                                                LEFT JOIN OCS_EQUIPAMENTO eq WITH (NOLOCK)ON ocs.CD_MOVIMENTO = eq.CD_MOVIMENTO
                                                                LEFT JOIN TAB_EQUIPAMENTO tab_e WITH (NOLOCK)ON eq.CD_EQUIPAMENTO = tab_e.CD_EQUIPAMENTO
                                                                WHERE ocs.CD_MOVIMENTO = rota.CD_MOVIMENTO
                                                                GROUP BY tab_e.NM_EQUIPAMENTO) agg)
                                                     ELSE ''
                                                 END AS rota_equipamento,
                                                 cast(
                                                        (SELECT sum(isnull(vol.VL_CUBAGEM, 0))
                                                         FROM OCS_EQUIPAMENTO_VOLUME vol
                                                         LEFT JOIN OCS_EQUIPAMENTO equ ON vol.CD_MOVIMENTO_EQUIPAMENTO = equ.CD_MOVIMENTO_EQUIPAMENTO
                                                         WHERE equ.CD_MOVIMENTO = rota.CD_MOVIMENTO) AS numeric(15, 3)) AS rota_peso_cubico,
                                                 cast(
                                                        (SELECT sum(isnull(vol.VL_PESO_BRUTO, 0))
                                                         FROM OCS_EQUIPAMENTO_VOLUME vol WITH (NOLOCK)
                                                         LEFT JOIN OCS_EQUIPAMENTO equ WITH (NOLOCK)ON vol.CD_MOVIMENTO_EQUIPAMENTO = equ.CD_MOVIMENTO_EQUIPAMENTO
                                                         WHERE equ.CD_MOVIMENTO = rota.CD_MOVIMENTO) AS numeric(15, 3)) AS rota_peso_bruto,

  (SELECT string_agg(nm_equipamento +' - '+ NR_FREE_TIME_VENDA +' Dias', ' / ')
   FROM
     (SELECT tab_e.NM_EQUIPAMENTO,
             cast((eq.NR_FREE_TIME_VENDA)AS varchar(5)) AS NR_FREE_TIME_VENDA
      FROM OCS_MOVIMENTO ocs WITH (NOLOCK)
      LEFT JOIN OCS_EQUIPAMENTO eq WITH (NOLOCK)ON ocs.CD_MOVIMENTO = eq.CD_MOVIMENTO
      LEFT JOIN TAB_EQUIPAMENTO tab_e WITH (NOLOCK)ON eq.CD_EQUIPAMENTO = tab_e.CD_EQUIPAMENTO
      WHERE ocs.CD_MOVIMENTO = rota.CD_MOVIMENTO
      GROUP BY tab_e.NM_EQUIPAMENTO,

               eq.NR_FREE_TIME_VENDA) agg) AS free_time ,
                                                 format(rota.DT_CONFIRMACAO_SAIDA, 'dd/MM/yyyy') AS embarque ,
                                                 format(rota.DT_CONFIRMACAO_CHEGADA, 'dd/MM/yyyy')AS cehgada ,
                                                 rota.DS_MOVIMENTO AS processo ,
                                                 prod.NM_PRODUTO AS produto ,
                                                 CASE
                                                     WHEN rota.CD_MODALIDADE_FRETE = 2 THEN desova.NM_PESSOA
                                                     ELSE NULL
                                                 END AS terminal_desova,
                                     
    OFT.DS_REFERENCIA_PESSOA AS referencia ,
   

  (SELECT top 1 pais.NM_PAIS
   FROM OCS_MOVIMENTO movimento WITH (NOLOCK)
   LEFT JOIN LOC_LOCALIDADE localidade WITH (NOLOCK)ON localidade.CD_LOCALIDADE = movimento.CD_DESTINO
   LEFT JOIN LOC_ESTADO estado WITH (NOLOCK)ON estado.CD_ESTADO = localidade.CD_ESTADO
   LEFT JOIN LOC_PAIS pais WITH (NOLOCK)ON pais.CD_PAIS = estado.CD_PAIS
   LEFT JOIN LOC_PAIS pais_cd_cidade WITH (NOLOCK)on pais_cd_cidade.CD_PAIS  = localidade.CD_PAIS
   WHERE movimento.CD_MOVIMENTO = rota.CD_MOVIMENTO )AS pais_destino ,
                                                 oe.QT_CONTAINER ,
                                                 OCS_OFERTA.DS_REFERENCIA_PESSOA AS referencia_cliente ,
                                                 NR_FREE_TIME_COMPRA ,
                                                 rota.DS_HOUSE ,
                                                 CLIENTE.NM_FANTASIA AS nm_cliente ,
                                                 clie.NM_FANTASIA AS cliente_em ,
                                                 cast(
                                                        (SELECT sum(isnull(vol.VL_CUBAGEM, 0))
                                                         FROM OCS_EQUIPAMENTO_VOLUME vol  WITH (NOLOCK)
                                                         LEFT JOIN OCS_EQUIPAMENTO equ WITH (NOLOCK)ON vol.CD_MOVIMENTO_EQUIPAMENTO = equ.CD_MOVIMENTO_EQUIPAMENTO
                                                         WHERE equ.CD_MOVIMENTO = rota.CD_MOVIMENTO) AS numeric(15, 3)) AS peso_cubico_nk ,
                                                 FORMAT(cast(
                                                               (SELECT sum(isnull(vol.VL_PESO_BRUTO, 0))
                                                                FROM OCS_EQUIPAMENTO_VOLUME vol WITH (NOLOCK)
                                                                LEFT JOIN OCS_EQUIPAMENTO equ WITH (NOLOCK)ON vol.CD_MOVIMENTO_EQUIPAMENTO = equ.CD_MOVIMENTO_EQUIPAMENTO
                                                                WHERE equ.CD_MOVIMENTO = rota.CD_MOVIMENTO) AS numeric(15, 3)), '###,###,##0.00') AS peso_bruto_nk ,

  (SELECT STRING_AGG(ncm.DS_NCM, '/ ')
   FROM OCS_NCM ncm WITH (NOLOCK)
   WHERE ncm.CD_MOVIMENTO = rota.CD_MOVIMENTO) AS ncm ,

  (SELECT TOP 1 serv.DS_OBS
   FROM OCS_SERVICOS serv WITH (NOLOCK)
   WHERE serv.CD_MOVIMENTO = rota.CD_MOVIMENTO
     AND CD_SERVICO = 46) AS coleta_inf_adic ,
                                                 destinom.NM_LOCALIDADE AS destino_mawb ,
                                                 FORMAT(cast(
                                                               (SELECT sum(isnull(vol.VL_PESO_BRUTO, 0))
                                                                FROM OCS_EQUIPAMENTO_VOLUME vol WITH (NOLOCK)
                                                                LEFT JOIN OCS_EQUIPAMENTO equ WITH (NOLOCK)ON vol.CD_MOVIMENTO_EQUIPAMENTO = equ.CD_MOVIMENTO_EQUIPAMENTO
                                                                WHERE equ.CD_MOVIMENTO = rota.CD_MOVIMENTO) AS numeric(15, 3)), '###,###,##0.00') AS total_peso_bruto ,
                                                 cast(
                                                        (SELECT sum(isnull(vol.VL_CUBAGEM, 0))
                                                         FROM OCS_EQUIPAMENTO_VOLUME vol WITH (NOLOCK)
                                                         LEFT JOIN OCS_EQUIPAMENTO equ WITH (NOLOCK)ON vol.CD_MOVIMENTO_EQUIPAMENTO = equ.CD_MOVIMENTO_EQUIPAMENTO
                                                         WHERE equ.CD_MOVIMENTO = rota.CD_MOVIMENTO) AS numeric(15, 3)) AS total_peso_cubico ,

  (SELECT top 1 isnull(vol.DS_MERCADORIA, '')
   FROM OCS_EQUIPAMENTO_VOLUME vol WITH (NOLOCK)
   LEFT JOIN OCS_EQUIPAMENTO equ WITH (NOLOCK)ON vol.CD_MOVIMENTO_EQUIPAMENTO = equ.CD_MOVIMENTO_EQUIPAMENTO
   WHERE equ.CD_MOVIMENTO = rota.CD_MOVIMENTO) AS commodity ,
                                                 entrega.NM_LOCALIDADE AS cidade_entrega ,
                                                 CASE
                                                     WHEN modal.CD_MODALIDADE = 2 THEN desova.NM_FANTASIA
                                                     ELSE ''
                                                 END AS desova_lcl ,

  (SELECT TOP 1 serv.DS_OBS
   FROM OCS_SERVICOS serv WITH (NOLOCK)
   WHERE serv.CD_MOVIMENTO = rota.CD_MOVIMENTO
     AND CD_SERVICO = 47) AS entrega_inf_adic ,
                                                 destino.nm_localidade + ' - ' + destino.CD_SISCOMEX AS destino_com_siscomex ,
                                                 destinom.NM_LOCALIDADE + ' - ' + destinom.CD_SISCOMEX AS destino_mawb_com_siscomex ,
                                                 CASE
                                                     WHEN (incoterm.CD_INCOTERM = 3) THEN origem.NM_LOCALIDADE + ' - ' + origem.CD_SISCOMEX
                                                     ELSE ''
                                                 END AS condicao_origem_embarque ,
                                                 desova.NM_FANTASIA AS terminal_desova_ ,
                                                 CASE
                                                     WHEN ID_MOSTRAR_ROTA = 1 THEN origem.NM_LOCALIDADE +'/'+
                                                            (SELECT STRING_AGG(d.NM_LOCALIDADE, '/')
                                                             FROM OCS_TRECHO tr WITH (NOLOCK)
                                                             LEFT JOIN LOC_LOCALIDADE d WITH (NOLOCK)ON d.CD_LOCALIDADE = tr.CD_LOCALIDADE
                                                             WHERE tr.CD_MOVIMENTO = rota.CD_MOVIMENTO ) + CASE
                                                                                                               WHEN destino.NM_LOCALIDADE = destinoF.NM_LOCALIDADE THEN '/' + isnull(destino.NM_LOCALIDADE, '')
                                                                                                               ELSE '/'+isnull(destino.NM_LOCALIDADE, '') +'/'+isnull(destinof.NM_LOCALIDADE, '')
                                                                                                           END
                                                     ELSE NULL
                                                 END AS rota ,
                                                 CASE
                                                     WHEN ID_MOSTRAR_AGENTE = 1 THEN Agente.NM_FANTASIA
                                                     ELSE NULL
                                                 END AS agente ,
                                                 CASE
                                                     WHEN ID_MOSTRAR_ARMADOR = 1 THEN Armador.NM_FANTASIA
                                                     ELSE NULL
                                                 END AS armador ,
	 cast(VL_TRANSIT_TIME_DE as varchar(4)) + isnull(' à ' + cast(VL_TRANSIT_TIME_ATE as varchar(4)),'') + ' days' AS ROTA_TRANSIT_TIME_VALOR_ING,

  (SELECT top 1 se.DS_OBS
   FROM OCS_SERVICOS se WITH (NOLOCK)
   LEFT JOIN OCS_MOVIMENTO m WITH (NOLOCK)ON m.CD_MOVIMENTO = se.CD_MOVIMENTO
   WHERE se.CD_SERVICO = 46
     AND m.CD_OFERTA = rota.CD_OFERTA) AS coleta ,

--		case when sg.CD_SERVICO = 28 then  '<table class="grade-informacoes-wrapper">
--    <tbody>
--        <tr class="informacoes">
--            <td>
--                <p style="font-size:13px;"> <span class="label">SEGURO DE CARGAS (opcional)</span></p>
--            </td>
--        </tr>
--        <tr class="informacoes">
--            <td>
--                <div class="grade-informacoes">
--                    -[TAB SEGURO OPCIONAL]- 
--                </div>
--            </td>
--        </tr>
--    </tbody>
--</table>' else '' end as seguro_opcional,

  (SELECT top 1 se.DS_OBS
   FROM OCS_SERVICOS se WITH (NOLOCK)
   LEFT JOIN OCS_MOVIMENTO m WITH (NOLOCK)ON m.CD_MOVIMENTO = se.CD_MOVIMENTO
   WHERE se.CD_SERVICO = 47
     AND m.CD_OFERTA = rota.CD_OFERTA) AS entrega ,
    

  (SELECT string_agg(FT_NO_DESTINO + ' Dias ', ' / ')
   FROM
     (SELECT cast((eq.FT_NO_DESTINO)AS varchar(5)) AS FT_NO_DESTINO
      FROM OCS_MOVIMENTO ocs WITH (NOLOCK)
      LEFT JOIN OCS_EQUIPAMENTO eq WITH (NOLOCK)ON ocs.CD_MOVIMENTO = eq.CD_MOVIMENTO
      LEFT JOIN TAB_EQUIPAMENTO tab_e WITH (NOLOCK)ON eq.CD_EQUIPAMENTO = tab_e.CD_EQUIPAMENTO
      WHERE ocs.CD_MOVIMENTO = rota.CD_MOVIMENTO
      GROUP BY eq.FT_NO_DESTINO,
               eq.cd_equipamento) agg) AS free_time_destino ,
                                                 rota.DS_INVOICE AS INVOICE
	
	,(SELECT top 1 isnull(pais.NM_PAIS, pais_destino.NM_PAIS)
	   FROM LOC_LOCALIDADE localidade WITH (NOLOCK)
		LEFT JOIN LOC_PAIS pais WITH (NOLOCK)on pais.CD_pais = localidade.CD_PAIS 
		LEFT JOIN loc_pais pais_destino WITH (NOLOCK)ON pais_destino.CD_PAIS = rota.CD_PAIS_DESTINO
	   WHERE localidade.CD_LOCALIDADE = rota.CD_DESTINO )AS pais_destino_fast

      -- case when sg.CD_SERVICO = 46 then


FROM OCS_MOVIMENTO rota WITH (NOLOCK)
LEFT JOIN LOC_LOCALIDADE origemE WITH (NOLOCK)ON origemE.CD_LOCALIDADE = rota.CD_ORIGEM_EMBARQUE
LEFT JOIN LOC_LOCALIDADE origem WITH (NOLOCK)ON origem.CD_LOCALIDADE = rota.CD_ORIGEM
LEFT JOIN LOC_LOCALIDADE destino WITH (NOLOCK)ON destino.CD_LOCALIDADE = rota.CD_DESTINO
LEFT JOIN LOC_LOCALIDADE destinoF WITH (NOLOCK)ON destinoF.CD_LOCALIDADE = rota.CD_CIDADE_ENTREGA
LEFT JOIN LOC_LOCALIDADE coleta WITH (NOLOCK)ON coleta.CD_LOCALIDADE = rota.CD_CIDADE_COLETA
LEFT JOIN LOC_LOCALIDADE entrega WITH (NOLOCK)ON entrega.CD_LOCALIDADE = rota.cd_cidade_entrega
LEFT JOIN LOC_ESTADO estado WITH (NOLOCK)ON estado.CD_ESTADO = entrega.CD_ESTADO
LEFT JOIN LOC_LOCALIDADE destinoDTA WITH (NOLOCK)ON destinoDTA.CD_LOCALIDADE = rota.cd_destino_dta
LEFT JOIN TAB_INCOTERM incoterm WITH (NOLOCK)ON incoterm.CD_INCOTERM = rota.CD_INCOTERM
LEFT JOIN LOC_PAIS pais_origem WITH (NOLOCK)ON pais_origem.CD_PAIS = rota.CD_PAIS_ORIGEM
LEFT JOIN LOC_PAIS pais_entrega WITH (NOLOCK)ON pais_entrega.CD_PAIS = rota.CD_PAIS_ENTREGA
LEFT  JOIN loc_pais pais_destino WITH (NOLOCK)ON pais_destino.CD_PAIS = rota.CD_PAIS_DESTINO
LEFT JOIN LOC_PAIS pais_coleta WITH (NOLOCK)ON pais_coleta.CD_PAIS = rota.CD_PAIS_COLETA
LEFT JOIN PES_PESSOA Armador WITH (NOLOCK)ON armador.CD_PESSOA =
  (SELECT OS.CD_FORNECEDOR
   FROM OCS_SERVICOS OS WITH (NOLOCK)
   WHERE os.CD_MOVIMENTO = ROTA.CD_MOVIMENTO
     AND OS.CD_SERVICO = 1)
LEFT JOIN TAB_MOEDA WITH (NOLOCK)ON ROTA.CD_MOEDA_INVOICE = TAB_MOEDA.CD_MOEDA
LEFT JOIN pes_pessoa agente WITH (NOLOCK)ON agente.CD_PESSOA = rota.CD_AGENTE
LEFT JOIN LOC_PAIS pais_agente WITH (NOLOCK)ON pais_agente.CD_PAIS = agente.cd_pais
LEFT JOIN LOC_ESTADO estado_agente WITH (NOLOCK)ON estado_agente.CD_ESTADO = agente.CD_ESTADO
LEFT JOIN LOC_LOCALIDADE agente_localidade WITH (NOLOCK)ON agente_localidade.CD_LOCALIDADE = agente.CD_CIDADE
LEFT JOIN OCS_OFERTA WITH (NOLOCK)ON rota.CD_OFERTA = OCS_OFERTA.CD_OFERTA
LEFT JOIN TAB_MOEDA mMerc WITH (NOLOCK)ON mMerc.CD_MOEDA = rota.CD_MOEDA_INVOICE
LEFT JOIN PES_PESSOA desova WITH (NOLOCK)ON desova.CD_PESSOA = rota.CD_TERMINAL_DESOVA
LEFT JOIN PRO_PRODUTO prod WITH (NOLOCK)ON prod.CD_PRODUTO = rota.CD_PRODUTO
LEFT JOIN OCS_EQUIPAMENTO oe WITH (NOLOCK)ON oe.CD_MOVIMENTO = rota.CD_MOVIMENTO
LEFT JOIN PES_PESSOA cliente WITH (NOLOCK)ON cliente.cd_pessoa = rota.CD_MOVIMENTO
LEFT JOIN OCS_OFERTA oft WITH (NOLOCK)ON rota.CD_OFERTA = oft.CD_OFERTA
LEFT JOIN pes_pessoa clie WITH (NOLOCK)ON clie.CD_PESSOA = oft.CD_PESSOA
LEFT JOIN LOC_LOCALIDADE destinom WITH (NOLOCK)ON destinom.CD_LOCALIDADE = rota.CD_DESTINO_MAWB
LEFT JOIN TAB_MODALIDADE modal WITH (NOLOCK)ON modal.CD_MODALIDADE = oft.CD_MODALIDADE_FRETE
--left join OCS_SERVICOS sg on rota.CD_MOVIMENTO = sg.CD_MOVIMENTO

WHERE rota.CD_movimento = 3499
ORDER BY NR_TRAFEGO ASC

--select CD_SERVICO,DS_SERVICOS FROM OCS_SERVICOS 
--left join TAB_SERVICOS serv WITH(NOLOCK) on serv.CD_SERVICOS = OCS_SERVICOS.CD_SERVICO



--select TOP 10 * FROM TAB_SERVICOS
--SELECT COLUMN_NAME, TABLE_NAME 
--FROM INFORMATION_SCHEMA.COLUMNS 
--WHERE COLUMN_NAME 
--like '%CD_SERVICOS%'

---- Mostra todos os relacionamentos da tabela pto_tacxa
--SELECT  
--    fk.name                AS ForeignKey,
--    tp.name                AS TabelaPai,
--    cp.name                AS ColunaPai,
--    tr.name                AS TabelaFilha,
--    cr.name                AS ColunaFilha
--FROM sys.foreign_keys fk
--INNER JOIN sys.foreign_key_columns fkc 
--    ON fk.object_id = fkc.constraint_object_id
--INNER JOIN sys.tables tp 
--    ON fkc.referenced_object_id = tp.object_id
--INNER JOIN sys.columns cp 
--    ON fkc.referenced_object_id = cp.object_id 
--   AND fkc.referenced_column_id = cp.column_id
--INNER JOIN sys.tables tr 
--    ON fkc.parent_object_id = tr.object_id
--INNER JOIN sys.columns cr 
--    ON fkc.parent_object_id = cr.object_id 
--   AND fkc.parent_column_id = cr.column_id
--WHERE tp.name = 'TAB_SERVICOS'   -- Tabela sendo referenciada
--   OR tr.name = 'TAB_SERVICOS'   -- Tabela que referencia
--ORDER BY TabelaPai, TabelaFilha;
