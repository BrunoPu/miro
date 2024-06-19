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