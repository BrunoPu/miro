WITH latest_date AS (
    SELECT year, month, day
    FROM sys_user_group_sot
    ORDER BY year DESC, month DESC, day DESC
    LIMIT 1
)
SELECT s.active, s.name, s.parent, s.sys_id, s.year, s.month, s.day
FROM sys_user_group_sot s
JOIN latest_date ld
ON s.year = ld.year AND s.month = ld.month AND s.day = ld.day;