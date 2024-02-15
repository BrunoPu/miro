SELECT 
    CASE 
        WHEN LEFT(NomeCertificado, 1) = '*' THEN SUBSTRING(NomeCertificado, 2, LEN(NomeCertificado) - 1)
        ELSE NomeCertificado
    END AS NomeCertificado_sem_prefixo
FROM TBFilaCertificadoAWSmanual
WHERE LEFT(NomeCertificado, 1) != '*'
   OR NomeCertificado IS NULL;
