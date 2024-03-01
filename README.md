-- Passo 1: Identificar registros duplicados na tabela de certificados
WITH CertificadosDuplicados AS (
    SELECT id, nomecertificado, arn,
           ROW_NUMBER() OVER (PARTITION BY nomecertificado, arn ORDER BY id) AS row_num
    FROM certificados
)
-- Passo 2: Excluir registros duplicados na tabela de certificados
DELETE FROM CertificadosDuplicados
WHERE row_num > 1;

-- Passo 3: Identificar IDs dos registros excluídos na tabela de certificados
SELECT id
INTO #IdsExcluidos
FROM CertificadosDuplicados;

-- Passo 4: Excluir registros correspondentes na tabela de responsáveis de certificados
DELETE FROM responsavel_certificados
WHERE idcertificado IN (SELECT id FROM #IdsExcluidos);

-- Passo 5: Excluir registros duplicados na tabela de certificados com base no ARN e no nome
DELETE FROM certificados
WHERE id IN (SELECT id FROM #IdsExcluidos);

-- Limpar tabela temporária
DROP TABLE #IdsExcluidos;
