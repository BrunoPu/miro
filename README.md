SELECT DISTINCT 
    REPLACE(m.nomecertificado, '*.', '') AS nomecertificado_sem_prefixo
FROM 
    tbcertificadoawsmanual m
INNER JOIN 
    tbcertificadoawslegado l ON REPLACE(m.nomecertificado, '*.', '') = REPLACE(l.certificado, '*.', '');
