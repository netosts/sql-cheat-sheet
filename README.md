# Cheat Sheet for SQL

## PostgreSQL
### Scripts
Get laravel request rules by columns data types:
```sql
SELECT 
    CONCAT(
        '''', COLUMN_NAME, ''' => [ ',
        CASE
            WHEN IS_NULLABLE = 'YES' THEN '''nullable'''
            ELSE '''required'''
        END,
        CASE 
            WHEN DATA_TYPE IN ('character varying', 'varchar', 'char', 'character') AND CHARACTER_MAXIMUM_LENGTH BETWEEN 0 AND 255 THEN CONCAT(', ''max:', CHARACTER_MAXIMUM_LENGTH, '''')
            when DATA_TYPE in ('int8', 'bigserial', 'bigint') then CONCAT(', ''', 'numeric', '''')
            ELSE CONCAT(', ''', DATA_TYPE, '''')
        END,
        ' ],'
    ) AS column_details
FROM 
    INFORMATION_SCHEMA.COLUMNS
WHERE 
    TABLE_NAME = 'tableName'
ORDER BY 
    ORDINAL_POSITION;
```

## MYSQL
### Scripts
Get the columns from a database table in a format that can be converted to a TypeScript interface:
```MYSQL
SELECT 
    CONCAT(COLUMN_NAME, 
        CASE
            WHEN IS_NULLABLE = 'YES' THEN '?'
            ELSE ''
        END, ':') AS name,
    CONCAT(
        CASE 
            WHEN DATA_TYPE IN ('varchar', 'char', 'date', 'character varying', 'timestamp', 'timestamp without time zone', 'text') THEN 'string'
            WHEN DATA_TYPE IN ('int', 'double', 'decimal', 'bigint') THEN 'number'
            WHEN DATA_TYPE IN ('tinyint') THEN 'boolean'
            WHEN DATA_TYPE IN ('json') THEN 'object'
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

Get the columns from a database table in a format that can be converted to a PHP DTO:
```MYSQL
SELECT 
    CONCAT(
        CASE 
            WHEN IS_NULLABLE = 'YES' THEN '?'
            ELSE ''
        END,
        CASE 
            WHEN DATA_TYPE IN ('varchar', 'char', 'date', 'character varying', 'timestamp', 'timestamp without time zone', 'text') THEN 'string'
            WHEN DATA_TYPE IN ('int', 'bigint') THEN 'int'
            WHEN DATA_TYPE IN ('double', 'decimal') THEN 'float'
            WHEN DATA_TYPE IN ('tinyint') THEN 'bool'
            WHEN DATA_TYPE IN ('json') THEN 'array'
            ELSE DATA_TYPE -- For other data types, just return the original SQL data type
        END
    ) AS type,
    CONCAT('$', COLUMN_NAME, ';') AS name
FROM 
    INFORMATION_SCHEMA.COLUMNS
WHERE 
    TABLE_NAME = 'YourTableName'
ORDER BY 
    ORDINAL_POSITION;
```
