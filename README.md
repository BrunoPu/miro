INSERT INTO #TempResponsaveis (idCertificado, AnalistaFuncional)
SELECT idCertificado,
       LEFT(value, CHARINDEX('-', value) - 1) AS AnalistaFuncional
FROM YourTableName -- Substitua "YourTableName" pelo nome correto da sua tabela
CROSS APPLY STRING_SPLIT(responsavel, ',');
        
