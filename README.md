SELECT DISTINCT 
    CASE 
        WHEN LEFT(REPLACE(m.nomecertificado, '*.', ''), 2) = '.'
        THEN SUBSTRING(REPLACE(m.nomecertificado, '*.', ''), 2, LEN(m.nomecertificado) - 1)
        ELSE m.nomecertificado
    END AS nomecertificado_sem_prefixo
FROM 
    tbFilacertificadoAWSmanual m
LEFT JOIN 
    tbCertificadosawsLegado l ON 
        CASE 
            WHEN LEFT(REPLACE(m.nomecertificado, '*.', ''), 2) = '.'
            THEN SUBSTRING(REPLACE(m.nomecertificado, '*.', ''), 2, LEN(m.nomecertificado) - 1)
            ELSE m.nomecertificado
        END = 
        CASE 
            WHEN LEFT(REPLACE(l.certificado, '*.', ''), 2) = '.'
            THEN SUBSTRING(REPLACE(l.certificado, '*.', ''), 2, LEN(l.certificado) - 1)
            ELSE l.certificado
        END
WHERE 
    l.certificado IS NULL;
