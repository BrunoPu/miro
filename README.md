INSERT INTO #IdsExcluidos (id)
SELECT id
FROM TBCertificadosAWSLegado
WHERE (CertificadoNome, arn) IN (
    SELECT CertificadoNome, arn
    FROM TBCertificadosAWSLegado
    GROUP BY CertificadoNome, arn
    HAVING COUNT(*) > 1
