 SELECT DISTINCT
            @id,
            CASE WHEN CHARINDEX('-', split.value) > 0 THEN LEFT(split.value, CHARINDEX('-', split.value) - 1) ELSE split.value END AS Nome,
            CASE WHEN CHARINDEX('-', split.value) > 0 THEN RIGHT(split.value, LEN(split.value) - CHARINDEX('-', split.value)) ELSE NULL END AS Funcional
            FROM TBFilaCertificadosAWSmanual C (NOLOCK)
            CROSS APPLY STRING_SPLIT(C.Responsavel, ',') AS split
            WHERE C.NomeCertificado = @certificadoNome OR @arn = C.ArnCertificado;
