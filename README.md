INSERT INTO TBAnalistacertificadosAMSLegado (idCertificado, AnalistaFuncional, AnalistaNome)
SELECT @id, 
       CASE WHEN CHARINDEX('-', split.value) > 0 THEN LEFT(split.value, CHARINDEX('-', split.value) - 1) ELSE split.value END AS Funcional,
       R.Responsavel -- Usando o nome retornado diretamente de TBFilaCertificadosAWSmanual
FROM STRING_SPLIT(@Responsaveis, ',') AS split
LEFT JOIN TBFilaCertificadosAWSmanual R ON CASE WHEN CHARINDEX('-', split.value) > 0 THEN LEFT(split.value, CHARINDEX('-', split.value) - 1) ELSE split.value END = R.Funcional
WHERE (R.NomeCertificado = @certificadoNome OR @arn = R.ArnCertificado) AND R.Responsavel IS NOT NULL;
