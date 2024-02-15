SELECT 
    -- Remove o prefixo '*.' se estiver presente
    CASE 
        WHEN LEFT(NomeCertificado, 2) = '*.' THEN SUBSTRING(NomeCertificado, 3, LEN(NomeCertificado) - 2) -- Se for '*.', remove '*.' e retorna o restante da string
        ELSE NomeCertificado -- Se não for '*.', retorna NomeCertificado sem fazer alterações
    END AS NomeCertificado_sem_prefixo,
    -- Seleciona a coluna dtsolicitacao
    dtsolicitacao
FROM TBFilaCertificadoAWSmanual;
