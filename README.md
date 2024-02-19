SELECT idCertificado,
       LEFT(value, CHARINDEX('-', value) - 1) AS AnalistaFuncional
FROM (
    SELECT idCertificado, value
    FROM YourTableName -- Substitua "YourTableName" pelo nome correto da sua tabela
    CROSS APPLY STRING_SPLIT(responsavel, ',')
) AS SplitValues
GROUP BY idCertificado, value;
