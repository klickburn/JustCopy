
-- For maximum scale (number of digits after the decimal point)
SELECT MAX(LENGTH(SUBSTR(TO_CHAR(column_name), INSTR(TO_CHAR(column_name), '.') + 1))) AS max_scale
FROM your_table
WHERE INSTR(TO_CHAR(column_name), '.') > 0;

-- For maximum precision (total number of digits)
SELECT MAX(LENGTH(REPLACE(TO_CHAR(column_name), '.', ''))) AS max_precision
FROM your_table;