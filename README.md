SELECT 
    *
FROM 
    (
        SELECT 
            *,
            ROW_NUMBER() OVER (PARTITION BY SupportGroupID ORDER BY UpdateDate DESC) AS rn
        FROM 
            SupportGroups
    ) AS LatestRecords
WHERE 
    rn = 1;


SELECT 
    SupportGroupID, Column1, Column2, UpdateDate -- Listar todas as colunas necessárias da tabela SupportGroups
FROM 
    (
        SELECT 
            SupportGroupID, Column1, Column2, UpdateDate,
            ROW_NUMBER() OVER (PARTITION BY SupportGroupID ORDER BY UpdateDate DESC) AS rn
        FROM 
            SupportGroups
    ) AS LatestRecords
WHERE 
    rn = 1;