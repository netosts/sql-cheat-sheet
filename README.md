# Cheat Sheet for SQL

## MYSQL
### Scripts
- Get the columns from a database table in a format that can be converted to a TypeScript interface:
```MYSQL
SELECT 
    CONCAT(COLUMN_NAME, 
        CASE
            WHEN IS_NULLABLE = 'YES' THEN '?'
            ELSE ''
        END, ':') AS name,
    CONCAT(
        CASE 
            WHEN DATA_TYPE IN ('varchar', 'char', 'date', 'timestamp') THEN 'string'
            WHEN DATA_TYPE IN ('int', 'tinyint', 'double') THEN 'number'
            ELSE DATA_TYPE -- For other data types, just return the original SQL data type
        END,
        ';'
    ) AS type
FROM 
    INFORMATION_SCHEMA.COLUMNS
WHERE 
    TABLE_NAME = 'YourTableName'
ORDER BY 
    ORDINAL_POSITION;
```
