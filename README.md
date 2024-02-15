SELECT DISTINCT 
    CASE 
        WHEN LEFT(m.nomecertificado, 2) = '*.'
        THEN SUBSTRING(m.nomecertificado, 3, LEN(m.nomecertificado) - 2)
        ELSE m.nomecertificado
    END AS nomecertificado_sem_prefixo
FROM 
    tbcertificadoawsmanual m
INNER JOIN 
    tbcertificadoawslegado l ON SUBSTRING(m.nomecertificado, 3, LEN(m.nomecertificado) - 2) = SUBSTRING(l.certificado, 3, LEN(l.certificado) - 2);
