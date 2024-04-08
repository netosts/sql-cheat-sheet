# Cheat Sheet for SQL

## MYSQL
### Scripts
- Get the columns from a database table in a format that can be converted to a TypeScript interface:
```MYSQL
SELECT 
    COLUMN_NAME AS name,
    CONCAT(DATA_TYPE, 
        CASE
            WHEN CHARACTER_MAXIMUM_LENGTH IS NOT NULL THEN CONCAT('(', CHARACTER_MAXIMUM_LENGTH, ')')
            ELSE ''
        END) AS type,
    CASE
        WHEN IS_NULLABLE = 'YES' THEN ' | null'
        ELSE ''
    END AS nullable
FROM 
    INFORMATION_SCHEMA.COLUMNS
WHERE 
    TABLE_NAME = 'YourTableName'
ORDER BY 
    ORDINAL_POSITION;

```
