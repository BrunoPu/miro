SELECT 
    c.idcertificado,
    rc.funcional,
    rc.nome_analista
FROM 
    certificado c
OUTER APPLY (
    SELECT TOP 5
        rc.funcional,
        rc.nome_analista
    FROM 
        responsavelcertificado rc
    WHERE 
        rc.idcertificado = c.idcertificado
    ORDER BY 
        rc.funcional
) AS rc;
