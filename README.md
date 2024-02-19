SELECT @id,
                   LEFT(split.value, CHARINDEX('-', split.value) - 1) AS AnalistaFuncional,
                   RIGHT(split.value, LEN(split.value) - CHARINDEX('-', split.value)) AS AnalistaNome
            FROM TBFilaCertificadosAWSmanual AS C
            CROSS APPLY STRING_SPLIT(C.Responsavel, ',') AS split
            WHERE (C.NomeCertificado = @certificadoNome OR @arn = C.ArnCertificado) AND split.value IS NOT NULL;
        
