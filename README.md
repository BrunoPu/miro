SELECT @id,
                   LEFT(SUBSTRING_INDEX(split.value, '-', 1), CHARINDEX('-', split.value + '-') - 1) AS AnalistaFuncional,
                   SUBSTRING_INDEX(split.value, '-', -1) AS AnalistaNome
            FROM TBFilaCertificadosAWSmanual AS C
            CROSS APPLY STRING_SPLIT(C.Responsavel, ',') AS split
            WHERE (C.NomeCertificado = @certificadoNome OR @arn = C.ArnCertificado) AND split.value IS NOT NULL;
        END;
