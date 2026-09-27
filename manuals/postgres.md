![postgres.png](../files/postgres.png)

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

### Остальные команды
```sql
----------Подключение к серверу------------
-- Подключиться к серверу под конкретным пользователем
psql -U postgres

-- Подключиться к конкретной базе данных
psql -U postgres -d mydb

-- Подключиться к удалённому серверу
psql -h localhost -p 5432 -U postgres -d mydb

----------Мета-команды psql: информация------------
-- Показать справку по всем мета-командам
\?

-- Показать справку по SQL-команде (например, ALTER TABLE)
\h ALTER TABLE

-- Список всех баз данных
\l

-- Список всех таблиц в текущей базе
\dt

-- Список таблиц с размерами
\dt+

-- Список всех схем
\dn

-- Список всех функций
\df

-- Список всех представлений
\dv

-- Список всех пользователей и ролей
\du

-- Показать структуру таблицы
\d table_name

-- Показать структуру с индексами и деталями
\d+ table_name

-- Показать код функции
\df+ function_name

----------Мета-команды psql: управление------------
-- Переключиться на другую базу данных
\c db_name

-- Выполнить SQL из файла
\i file.sql

-- Показать историю команд
\s

-- Сохранить историю в файл
\s history.txt

-- Выполнить предыдущую команду
\g

-- Выйти из psql
\q

----------Мета-команды psql: вывод------------
-- Включить расширенный вывод (для широких таблиц)
\x

-- Включить отображение времени выполнения запроса
\timing

-- Переключить выравнивание столбцов
\a

-- Форматировать вывод в HTML
\H

----------Управление базами данных------------
-- Создать базу данных
CREATE DATABASE db_name;

-- Создать базу данных, если не существует
CREATE DATABASE IF NOT EXISTS db_name;

-- Удалить базу данных
DROP DATABASE db_name;

-- Удалить базу данных, если существует
DROP DATABASE IF EXISTS db_name;

----------Управление таблицами: создание------------
-- Создать таблицу с автоинкрементным первичным ключом
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    username VARCHAR(50) NOT NULL,
    email VARCHAR(255) NOT NULL,
    created_at TIMESTAMP DEFAULT NOW()
);

-- Создать временную таблицу
CREATE TEMP TABLE temp_users (
    id INT,
    name TEXT
);

-- Создать таблицу, если не существует
CREATE TABLE IF NOT EXISTS users (...);

----------Управление таблицами: изменение------------
-- Добавить столбец
ALTER TABLE users ADD COLUMN age INT;

-- Удалить столбец
ALTER TABLE users DROP COLUMN age;

-- Переименовать столбец
ALTER TABLE users RENAME COLUMN username TO login;

-- Изменить тип данных столбца
ALTER TABLE users ALTER COLUMN age TYPE BIGINT;

-- Установить значение по умолчанию
ALTER TABLE users ALTER COLUMN age SET DEFAULT 0;

-- Удалить значение по умолчанию
ALTER TABLE users ALTER COLUMN age DROP DEFAULT;

-- Добавить первичный ключ
ALTER TABLE users ADD PRIMARY KEY (id);

-- Удалить ограничение первичного ключа
ALTER TABLE users DROP CONSTRAINT users_pkey;

-- Переименовать таблицу
ALTER TABLE users RENAME TO app_users;

----------Управление таблицами: удаление------------
-- Удалить таблицу
DROP TABLE users;

-- Удалить таблицу и зависимые объекты
DROP TABLE users CASCADE;

-- Удалить таблицу, если существует
DROP TABLE IF EXISTS users;

----------Управление ролями и пользователями------------
-- Создать роль
CREATE ROLE role_name;

-- Создать роль с логином и паролем
CREATE ROLE username NOINHERIT LOGIN PASSWORD 'password';

-- Создать пользователя (псевдоним для CREATE ROLE с LOGIN)
CREATE USER username PASSWORD 'password';

-- Изменить роль текущей сессии
SET ROLE new_role;

-- Разрешить role_1 использовать роль role_2
GRANT role_2 TO role_1;

-- Выдать право на подключение к базе
GRANT CONNECT ON DATABASE db_name TO username;

-- Выдать все привилегии на базу
GRANT ALL PRIVILEGES ON DATABASE db_name TO username;

-- Выдать право на SELECT для таблицы
GRANT SELECT ON table_name TO username;

-- Выдать право на INSERT, UPDATE, DELETE
GRANT INSERT, UPDATE, DELETE ON table_name TO username;

-- Отозвать привилегии
REVOKE ALL PRIVILEGES ON DATABASE db_name FROM PUBLIC;

-- Создать роль для группы пользователей
CREATE ROLE lab_tech;

-- Назначить роль пользователю
GRANT lab_tech TO lab_user1;

----------Управление схемами------------
-- Создать схему
CREATE SCHEMA schema_name;

-- Удалить схему
DROP SCHEMA schema_name CASCADE;

-- Отозвать право на создание объектов в public у всех
REVOKE CREATE ON SCHEMA public FROM PUBLIC;

----------Управление индексами------------
-- Создать индекс
CREATE INDEX idx_name ON table_name (column_name);

-- Создать уникальный индекс
CREATE UNIQUE INDEX idx_name ON table_name (column_name);

-- Создать индекс без блокировки записей (для production)
CREATE INDEX CONCURRENTLY idx_name ON table_name (column_name);

-- Удалить индекс
DROP INDEX idx_name;

----------Запросы данных------------
-- Выбрать все данные из таблицы
SELECT * FROM table_name;

-- Выбрать конкретные столбцы
SELECT column1, column2 FROM table_name;

-- Выбрать уникальные строки
SELECT DISTINCT column1 FROM table_name;

-- Выбрать с условием
SELECT * FROM table_name WHERE condition;

-- Выбрать с псевдонимом столбца
SELECT column1 AS alias_name FROM table_name;

-- Выбрать с LIKE
SELECT * FROM table_name WHERE column LIKE '%value%';

-- Выбрать с BETWEEN
SELECT * FROM table_name WHERE column BETWEEN low AND high;

-- Выбрать с IN
SELECT * FROM table_name WHERE column IN (value1, value2);

-- Ограничить количество строк
SELECT * FROM table_name LIMIT 10 OFFSET 20;

-- Сортировка
SELECT * FROM table_name ORDER BY column_name ASC;

-- Группировка
SELECT column1, COUNT(*) FROM table_name GROUP BY column1;

-- Фильтрация групп
SELECT column1, COUNT(*) FROM table_name GROUP BY column1 HAVING COUNT(*) > 1;

-- Подсчёт строк
SELECT COUNT(*) FROM table_name;

----------Объединения (JOIN)------------
-- INNER JOIN
SELECT * FROM table1 INNER JOIN table2 ON table1.id = table2.table1_id;

-- LEFT JOIN
SELECT * FROM table1 LEFT JOIN table2 ON table1.id = table2.table1_id;

-- RIGHT JOIN
SELECT * FROM table1 RIGHT JOIN table2 ON table1.id = table2.table1_id;

-- FULL OUTER JOIN
SELECT * FROM table1 FULL OUTER JOIN table2 ON table1.id = table2.table1_id;

-- CROSS JOIN
SELECT * FROM table1 CROSS JOIN table2;

----------Модификация данных------------
-- Вставить одну строку
INSERT INTO table_name (column1, column2) VALUES (value1, value2);

-- Вставить несколько строк
INSERT INTO table_name (column1, column2) VALUES
    (value1, value2),
    (value3, value4);

-- Вставить с возвратом сгенерированного значения
INSERT INTO table_name (column1) VALUES (value1) RETURNING id;

-- Обновить все строки
UPDATE table_name SET column1 = value1;

-- Обновить строки с условием
UPDATE table_name SET column1 = value1 WHERE condition;

-- Удалить все строки
DELETE FROM table_name;

-- Удалить строки с условием
DELETE FROM table_name WHERE condition;

----------Транзакции------------
-- Начать транзакцию
BEGIN;

-- Зафиксировать изменения
COMMIT;

-- Откатить изменения
ROLLBACK;

-- Создать точку сохранения
SAVEPOINT savepoint_name;

-- Откатиться к точке сохранения
ROLLBACK TO SAVEPOINT savepoint_name;

----------Представления (Views)------------
-- Создать представление
CREATE VIEW view_name AS SELECT * FROM table_name WHERE condition;

-- Создать или заменить представление
CREATE OR REPLACE VIEW view_name AS SELECT ...;

-- Создать материализованное представление
CREATE MATERIALIZED VIEW view_name AS SELECT ...;

-- Обновить материализованное представление
REFRESH MATERIALIZED VIEW CONCURRENTLY view_name;

-- Удалить представление
DROP VIEW IF EXISTS view_name;

-- Удалить материализованное представление
DROP MATERIALIZED VIEW view_name;

-- Переименовать представление
ALTER VIEW view_name RENAME TO new_name;

----------Резервное копирование и восстановление------------
-- Создать SQL-дамп базы
pg_dump -U db_admin db_name > db_backup.sql

-- Создать сжатый custom-формат дамп
pg_dump -U db_admin -Fc db_name > db_backup.dump

-- Восстановить SQL-дамп
psql -U db_admin -d db_name -f db_backup.sql

-- Восстановить custom-формат дамп
pg_restore -U db_admin -d db_name db_backup.dump

-- Создать дамп всех баз
pg_dumpall -U db_admin > cluster_backup.sql

-- Восстановить все базы
psql -U db_admin -f cluster_backup.sql

----------Анализ производительности------------
-- Показать план запроса
EXPLAIN SELECT * FROM table_name WHERE condition;

-- Показать план и выполнить запрос
EXPLAIN ANALYZE SELECT * FROM table_name WHERE condition;

-- Собрать статистику
ANALYZE table_name;

-- Собрать статистику для всех таблиц
VACUUM ANALYZE;

----------Настройки сервера (postgresql.conf)------------
-- Адреса для прослушивания подключений (по умолчанию localhost)
listen_addresses = 'localhost'

-- Порт сервера (по умолчанию 5432)
port = 5432

-- Максимальное число одновременных подключений (по умолчанию 100)
max_connections = 100

-- Зарезервированные слоты для суперпользователей (по умолчанию 3)
superuser_reserved_connections = 3

-- Каталог данных (только при старте сервера)
data_directory = '/var/lib/postgresql/data'

-- Файл основной конфигурации
config_file = 'postgresql.conf'

-- Файл аутентификации
hba_file = 'pg_hba.conf'

-- Файл маппинга пользователей
ident_file = 'pg_ident.conf'

-- Перечитать конфигурацию без перезапуска
-- pg_ctl reload
```