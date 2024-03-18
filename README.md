
SELECT
    MAX(LEN(CAST(your_column AS VARCHAR) - CHARINDEX('.', CAST(your_column AS VARCHAR)) - 1)) AS max_scale,
    MAX(LEN(REPLACE(CAST(your_column AS VARCHAR), '.', ''))) AS max_precision
FROM
    your_table
WHERE
    CHARINDEX('.', CAST(your_column AS VARCHAR)) > 0