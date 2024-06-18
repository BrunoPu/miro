SELECT s.active, s.name, s.parente, s.sys_id, CONCAT(s.year, '-', s.month, '-', s.day) AS DT
FROM sys_user_group_sot s
WHERE CONCAT(s.year, '-', s.month, '-', s.day) = (
    SELECT MAX(CONCAT(year, '-', month, '-', day))
    FROM sys_user_group_sot
);