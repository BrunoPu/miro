 -- Concatenar responsáveis
            DECLARE @Responsaveis VARCHAR(MAX);
            SELECT @Responsaveis = COALESCE(@Responsaveis + ',', '') + Responsavel
            FROM TBFilaCertificadosAWSmanual (NOLOCK)
            WHERE NomeCertificado = @certificadoNome OR @arn = ArnCertificado;

            -- Inserir na tabela de analistas
            INSERT INTO TBAnalistacertificadosAMSLegado (idCertificado, AnalistaFuncional, AnalistaNome)
            SELECT @id, 
                   CASE WHEN CHARINDEX('-', split.value) > 0 THEN LEFT(split.value, CHARINDEX('-', split.value) - 1) ELSE split.value END AS Funcional,
                   R.Nome
            FROM STRING_SPLIT(@Responsaveis, ',') AS split
            LEFT JOIN TBReflector R ON CASE WHEN CHARINDEX('-', split.value) > 0 THEN LEFT(split.value, CHARINDEX('-', split.value) - 1) ELSE split.value END = R.Funcional;
        
