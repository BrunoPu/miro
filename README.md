WITH LatestRecords AS (
    SELECT 
        *,
        ROW_NUMBER() OVER (PARTITION BY GroupID ORDER BY UpdateDate DESC) AS rn
    FROM 
        YourTable
)
SELECT 
    *
FROM 
    LatestRecords
WHERE 
    rn = 1;