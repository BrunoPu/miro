-- Passo 3: Identificar IDs dos registros excluídos na tabela de certificados
CREATE TABLE #IdsExcluidos (
    id INT
);

INSERT INTO #IdsExcluidos (id)
SELECT DISTINCT c1.id
FROM TBCertificadosAWSLegado c1
INNER JOIN (
    SELECT CertificadoNome, arn
    FROM TBCertificadosAWSLegado
    GROUP BY CertificadoNome, arn
    HAVING COUNT(*) > 1
) c2 ON c1.CertificadoNome = c2.CertificadoNome AND c1.arn = c2.arn;

-- Passo 4: Excluir registros correspondentes na tabela de responsáveis de certificados
DELETE FROM TBAnalistaCertificadoAWSLegado
WHERE idcertificado IN (SELECT id FROM #IdsExcluidos);

-- Passo 5: Excluir registros duplicados na tabela de certificados com base no ARN e no nome
DELETE FROM TBCertificadosAWSLegado
WHERE id IN (SELECT id FROM #IdsExcluidos);

-- Limpar tabela temporária
DROP TABLE #IdsExcluidos;
