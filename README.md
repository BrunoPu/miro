INSERT INTO TBAnalistacertificadosAMSLegado (idCertificado, AnalistaNome, AnalistaFuncional)
SELECT DISTINCT
    @id AS idCertificado,
    SUBSTRING_INDEX(SUBSTRING_INDEX(split.value, '-', -1), ',', 1) AS AnalistaNome,
    SUBSTRING_INDEX(split.value, '-', 1) AS AnalistaFuncional
FROM
    TBFilaCertificadosAWSmanual C (NOLOCK)
CROSS APPLY
    STRING_SPLIT(C.Responsavel, ',') AS split
WHERE
    (C.NomeCertificado = @certificadoNome OR @arn = C.ArnCertificado);
