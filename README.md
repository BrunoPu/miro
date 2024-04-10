SELECT 
    c.idcertificado,
    c.nome,
    c.numero_serie
FROM 
    certificado c
LEFT JOIN 
    responsavelcertificado rc ON c.idcertificado = rc.idcertificado
WHERE 
    rc.idcertificado IS NULL;
