
SELECT active, name, parent, sys_id, year, month, day
FROM sys_user_group_sot
WHERE (year, month, day) IN (
    SELECT year, month, day
    FROM sys_user_group_sot
    ORDER BY year DESC, month DESC, day DESC
    LIMIT 1
);