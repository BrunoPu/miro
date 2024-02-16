DECLARE @squad varchar(max)

SELECT @squad = GrupodeSuporte
FROM (
    SELECT DISTINCT TOP(1) GrupodeSuporte
    FROM @tblegado L
    INNER JOIN TBContasAWS Con ON C.IDconta = L.accountId
    INNER JOIN TBReflector R ON R.Squad = Con.GrupodeSuporte
) AS Subquery;

-- Agora você pode usar a variável @squad conforme necessário
