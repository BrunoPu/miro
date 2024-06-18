SELECT active, name, parent, sys_id, CONCAT(year, '-', month, '-', day) AS date
FROM sys_user_group_sot
WHERE CONCAT(year, '-', month, '-', day) = (
    SELECT MAX(CONCAT(year, '-', month, '-', day))
    FROM sys_user_group_sot
);