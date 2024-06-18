SELECT active, name, parent, sys_id, year, month, day
FROM sys_user_group_sot
WHERE CONCAT(year, '-', LPAD(month, 2, '0'), '-', LPAD(day, 2, '0')) = (
    SELECT MAX(CONCAT(year, '-', LPAD(month, 2, '0'), '-', LPAD(day, 2, '0')))
    FROM sys_user_group_sot
);