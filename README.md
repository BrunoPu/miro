SELECT
    @id AS idCertificado,
    LEFT(split.value, CHARINDEX('-', split.value) - 1) AS AnalistaFuncional,
    SUBSTRING(split.value, CHARINDEX('-', split.value) + 1, LEN(split.value) - CHARINDEX('-', split.value)) AS AnalistaNome
FROM
    TBFilaCertificadosAWSmanual C (NOLOCK)
CROSS APPLY
    STRING_SPLIT(C.Responsavel, ',') AS split
WHERE
    (C.NomeCertificado = @certificadoNome OR @arn = C.ArnCertificado)
GROUP BY
    C.NomeCertificado, C.Ambiente;
