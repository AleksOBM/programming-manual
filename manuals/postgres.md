# PostgreSQL

### Удалить все таблицы

```sql
DO $$ 
DECLARE
    r RECORD;
BEGIN
    FOR r IN (
        SELECT schemaname, tablename 
        FROM pg_tables 
        WHERE schemaname = 'public'
    ) LOOP
        EXECUTE 'DROP TABLE IF EXISTS ' 
            || quote_ident(r.schemaname) || '.' 
            || quote_ident(r.tablename) || ' CASCADE';
    END LOOP;
END $$;
```
