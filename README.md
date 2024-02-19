DECLARE @responsavel TABLE (
    idcertificado INT,
    AnalistaFuncional VARCHAR(MAX),
    AnalistaNome VARCHAR(300),
    dtcadastro DATE
)

INSERT INTO @responsavel (idcertificado, AnalistaFuncional, AnalistaNome)
SELECT
    C.id,
    Funcional,
    R.Nome
FROM (
    SELECT DISTINCT
        C.id,
        CASE WHEN CHARINDEX('-', split.value) > 0 THEN LEFT(split.value, CHARINDEX('-', split.value) - 1) ELSE split.value END AS Funcional
    FROM
        TBCertificados AWSAPI C (NOLOCK)
    CROSS APPLY
        STRING_SPLIT(C.analista_funcional, ',') AS split
    WHERE
        @nome = C.nome
) AS Funcionais
LEFT JOIN
    TBReflector R ON Funcionais.Funcional = R.Funcional
