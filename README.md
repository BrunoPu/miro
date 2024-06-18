WITH latest_date AS (
    SELECT MAX(CONCAT(year, '-', LPAD(month, 2, '0'), '-', LPAD(day, 2, '0'))) AS max_date
    FROM sys_user_group_sot
)
SELECT active, name, parent, sys_id, year, month, day
FROM sys_user_group_sot
WHERE CONCAT(year, '-', LPAD(month, 2, '0'), '-', LPAD(day, 2, '0')) = (
    SELECT max_date FROM latest_date
);


SELECT active, name, parent, sys_id, CONCAT(year, '-', month, '-', day) AS date
FROM sys_user_group_sot
WHERE CONCAT(year, '-', month, '-', day) = (
    SELECT MAX(CONCAT(year, '-', month, '-', day))
    FROM sys_user_group_sot
);