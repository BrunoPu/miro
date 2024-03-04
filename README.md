DELETE FROM analista
WHERE (idcertificado) IN (
    SELECT idcertificado
    FROM analista
    GROUP BY idcertificado 
    HAVING COUNT(*) > 100
);
