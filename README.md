-- Passo 1: Identificar registros duplicados na tabela de certificados
WITH CertificadosDuplicados AS (
    SELECT id, CertificadoNome, arn,
           ROW_NUMBER() OVER (PARTITION BY CertificadoNome, arn ORDER BY id) AS row_num
    FROM TBCertificadosAWSLegado
)
-- Passo 2: Excluir registros duplicados na tabela de certificados
DELETE FROM CertificadosDuplicados
WHERE row_num > 1;

-- Passo 3: Identificar IDs dos registros excluídos na tabela de certificados
CREATE TABLE #IdsExcluidos (
    id INT
);

INSERT INTO #IdsExcluidos (id)
SELECT id
FROM TBCertificadosAWSLegado
WHERE (CertificadoNome, arn) IN (
    SELECT CertificadoNome, arn
    FROM TBCertificadosAWSLegado
    GROUP BY CertificadoNome, arn
    HAVING COUNT(*) > 1
);

-- Passo 4: Excluir registros correspondentes na tabela de responsáveis de certificados
DELETE FROM TBAnalistaCertificadoAWSLegado
WHERE idcertificado IN (SELECT id FROM #IdsExcluidos);

-- Passo 5: Excluir registros duplicados na tabela de certificados com base no ARN e no nome
DELETE FROM TBCertificadosAWSLegado
WHERE id IN (SELECT id FROM #IdsExcluidos);

-- Limpar tabela temporária
DROP TABLE #IdsExcluidos;
