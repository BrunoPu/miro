INSERT INTO TargetTable (col1, col2, ..., colN) -- lista todas as colunas necessárias para inserção
SELECT 
    col1, col2, ..., colN
FROM (
    SELECT 
        col1, col2, ..., colN,
        ROW_NUMBER() OVER (PARTITION BY SupportGroupID ORDER BY UpdateDate DESC) AS rn
    FROM 
        SourceTable
) AS RecentRecords
WHERE rn = 1;