WITH CertificadoResponsaveis AS (
    SELECT 
        c.idcertificado,
        rc.funcional,
        rc.nome_analista,
        ROW_NUMBER() OVER (PARTITION BY c.idcertificado ORDER BY rc.funcional) AS rank
    FROM 
        certificado c
    INNER JOIN 
        responsavelcertificado rc ON c.idcertificado = rc.idcertificado
)
SELECT 
    idcertificado,
    funcional,
    nome_analista
FROM 
    CertificadoResponsaveis
WHERE 
    rank <= 5;
