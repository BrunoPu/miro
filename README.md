CREATE VIEW NomeDaSuaView AS
WITH AnalistaFuncionalCTE AS (
    SELECT
        C.idCertificado,
        SUBSTRING((SELECT TOP 1 AnalistaFuncional
                   FROM TBCertificadosAnalistas (NOLOCK) P
                   WHERE P.idCertificado = C2.idCertificado
                   FOR XML PATH ('')), 1, 1000) AS AnalistaFuncional
    FROM TBCertificadosAnalistas (NOLOCK) C2
),
AnalistaNomeCTE AS (
    SELECT
        C.idCertificado,
        SUBSTRING((SELECT TOP 1 AnalistaFuncional
                   FROM TBCertificadosAnalistas (NOLOCK) P
                   WHERE P.idCertificado = C2.idCertificado
                   FOR XML PATH ('')), 1, 1000) AS AnalistaNome
    FROM TBCertificadosAnalistas (NOLOCK) C2
),
SquadCTE AS (
    SELECT
        C.idCertificado,
        SUBSTRING((SELECT TOP 1 Nomes
                   FROM TBCertificadosSquad (NOLOCK) P
                   WHERE P.idCertificado = C2.idCertificado
                   FOR XML PATH ('')), 1, 1000) AS Squad
    FROM TBCertificadosSquad (NOLOCK) C2
),
ComunidadeCTE AS (
    SELECT
        C.idCertificado,
        SUBSTRING((SELECT TOP 1 Nomes
                   FROM TBCertificadosComunidade (NOLOCK) P
                   WHERE P.idCertificado = C2.idCertificado
                   FOR XML PATH ('')), 1, 1000) AS Comunidade
    FROM TBCertificadosComunidade (NOLOCK) C2
),
EmissorCTE AS (
    SELECT
        C.idEmissor,
        SUBSTRING((SELECT TOP 1 P.DescricaoEmissor
                   FROM TBCertificadosEmissor (NOLOCK) P
                   WHERE P.idEmissor = C2.idEmissor
                   FOR XML PATH ('')), 1, 1000) AS Emissor
    FROM TBCertificadosEmissor (NOLOCK) C2
)
SELECT
    C.Id,
    C.NomeCertificado,
    C.NumeroSerie,
    C.MultidominioSAN,
    C.Extensao,
    C.Ambiente,
    C.dtEmissao,
    C.dtVencimento,
    C.Status,
    C.CAInternaExterna,
    C.Balanceador,
    C.Sigla,
    LTRIM(CASE WHEN A.identificado IS NULL THEN C.AnalistaNome ELSE AF.AnalistaFuncional END) AS [AnalistaFuncional],
    LTRIM(CASE WHEN A.identificado IS NULL THEN C.AnalistaNome ELSE AN.AnalistaNome END) AS [AnalistaNome],
    LTRIM(SQ.Squad) AS Squad,
    LTRIM(COM.Comunidade) AS Comunidade,
    C.Aplicacao,
    EM.Emissor,
    SG.name,
    C.SupportGroup_sys_id,
    C.Servidores
FROM
    TBCertificados C (NOLOCK)
    LEFT JOIN TBSupportGroup SG (NOLOCK) ON SG.SYS_id = C.SupportGroup_sys_id
    LEFT JOIN AnalistaFuncionalCTE AF ON AF.idCertificado = C.id
    LEFT JOIN AnalistaNomeCTE AN ON AN.idCertificado = C.id
    LEFT JOIN SquadCTE SQ ON SQ.idCertificado = C.id
    LEFT JOIN ComunidadeCTE COM ON COM.idCertificado = C.id
    LEFT JOIN EmissorCTE EM ON EM.idEmissor = C.idEmissor
ORDER BY
    C.dtVencimento;