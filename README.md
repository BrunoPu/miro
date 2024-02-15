SELECT m.* -- Seleciona todas as colunas da tabela tbFilacertificadoAWSmanual
FROM tbFilacertificadoAWSmanual m -- Seleciona da tabela tbFilacertificadoAWSmanual, renomeando-a como 'm'
LEFT JOIN tbCertificadosawsLegado l ON -- Realiza uma junção com a tabela tbCertificadosawsLegado, renomeando-a como 'l'
    CASE 
        WHEN LEFT(m.nomecertificado, 2) = '*.' -- Verifica se o valor tem o prefixo '*.' na tabela tbFilacertificadoAWSmanual
        THEN SUBSTRING(m.nomecertificado, 3, LEN(m.nomecertificado) - 2) -- Remove o prefixo '*.' do valor na tabela tbFilacertificadoAWSmanual
        ELSE m.nomecertificado -- Se não tiver o prefixo '*.' na tabela tbFilacertificadoAWSmanual, mantém o valor original
    END = 
    CASE 
        WHEN LEFT(l.certificado, 2) = '*.' -- Verifica se o valor tem o prefixo '*.' na tabela tbCertificadosawsLegado
        THEN SUBSTRING(l.certificado, 3, LEN(l.certificado) - 2) -- Remove o prefixo '*.' do valor na tabela tbCertificadosawsLegado
        ELSE l.certificado -- Se não tiver o prefixo '*.' na tabela tbCertificadosawsLegado, mantém o valor original
    END
WHERE 
    l.certificado IS NULL -- Filtra os resultados para incluir apenas os valores da tabela tbFilacertificadoAWSmanual que não têm correspondência na tabela tbCertificadosawsLegado
    OR l.certificado IS NULL; -- Considera todos os valores da tabela tbCertificadosawsLegado, uma vez que o LEFT JOIN poderá resultar em valores nulos nesta tabela quando não houver correspondência
