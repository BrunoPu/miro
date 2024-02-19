  SELECT DISTINCT
                @id,
                CASE WHEN CHARINDEX('-', split.value) > 0 THEN LEFT(split.value, CHARINDEX('-', split.value) - 1) ELSE split.value END AS Funcional,
                R.Nome
            FROM TBFilaCertificadosAWSmanual C (NOLOCK)
            CROSS APPLY STRING_SPLIT(C.Responsavel, ',') AS split
            LEFT JOIN TBReflector R ON CASE WHEN CHARINDEX('-', split.value) > 0 THEN LEFT(split.value, CHARINDEX('-', split.value) - 1) ELSE split.value END = R.Funcional
            WHERE C.NomeCertificado = @certificadoNome OR @arn = C.ArnCertificado;
GROUP BY
    C.NomeCertificado, C.Ambiente;
