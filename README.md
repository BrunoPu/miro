DELETE a
FROM analista a
JOIN (
    SELECT idcertificado
    FROM analista
    GROUP BY idcertificado 
    HAVING COUNT(*) > 100
) b ON a.idcertificado = b.idcertificado;
