```bash
➜  ~ psql -U postgres -d practice_db          
Password for user postgres: 
psql (16.14 (Ubuntu 16.14-0ubuntu0.24.04.1))
Type "help" for help.

practice_db=# \i /home/nesk/projects/e-soft/e-sosft-hw-8/01_create_table.sql
psql:/home/nesk/projects/e-soft/e-sosft-hw-8/01_create_table.sql:7: NOTICE:  table "order_items" does not exist, skipping
DROP TABLE
psql:/home/nesk/projects/e-soft/e-sosft-hw-8/01_create_table.sql:8: NOTICE:  table "orders" does not exist, skipping
DROP TABLE
psql:/home/nesk/projects/e-soft/e-sosft-hw-8/01_create_table.sql:9: NOTICE:  table "products" does not exist, skipping
DROP TABLE
psql:/home/nesk/projects/e-soft/e-sosft-hw-8/01_create_table.sql:10: NOTICE:  table "categories" does not exist, skipping
DROP TABLE
psql:/home/nesk/projects/e-soft/e-sosft-hw-8/01_create_table.sql:11: NOTICE:  table "users" does not exist, skipping
DROP TABLE
CREATE TABLE
CREATE TABLE
CREATE TABLE
CREATE TABLE
CREATE TABLE
practice_db=# \dt
            List of relations
 Schema |    Name     | Type  |  Owner   
--------+-------------+-------+----------
 public | categories  | table | postgres
 public | order_items | table | postgres
 public | orders      | table | postgres
 public | products    | table | postgres
 public | users       | table | postgres
(5 rows)

practice_db=# \d users
                                        Table "public.users"
   Column   |            Type             | Collation | Nullable |              Default              
------------+-----------------------------+-----------+----------+-----------------------------------
 id         | integer                     |           | not null | nextval('users_id_seq'::regclass)
 email      | character varying(255)      |           | not null | 
 username   | character varying(100)      |           | not null | 
 role       | character varying(20)       |           | not null | 'customer'::character varying
 created_at | timestamp without time zone |           | not null | now()
Indexes:
    "users_pkey" PRIMARY KEY, btree (id)
    "users_email_key" UNIQUE CONSTRAINT, btree (email)
Referenced by:
    TABLE "orders" CONSTRAINT "orders_user_id_fkey" FOREIGN KEY (user_id) REFERENCES users(id)

practice_db=# \i /home/nesk/projects/e-soft/e-sosft-hw-8/02_insert.sql
INSERT 0 5
INSERT 0 4
INSERT 0 10
INSERT 0 6
INSERT 0 9
practice_db=# select * from users;
 id |      email       | username |   role   |         created_at         
----+------------------+----------+----------+----------------------------
  1 | alice@mail.com   | alice    | customer | 2026-05-26 10:32:25.945879
  2 | bob@mail.com     | bob      | customer | 2026-05-26 10:32:25.945879
  3 | charlie@mail.com | charlie  | admin    | 2026-05-26 10:32:25.945879
  4 | diana@mail.com   | diana    | customer | 2026-05-26 10:32:25.945879
  5 | eve@mail.com     | eve      | customer | 2026-05-26 10:32:25.945879
(5 rows)

practice_db=# \i /home/nesk/projects/e-soft/e-sosft-hw-8/03_select.sql
 id |          name          |            description            |  price   | in_stock | category_id |        created_at         
----+------------------------+-----------------------------------+----------+----------+-------------+---------------------------
  1 | Смартфон X10           | Флагманский смартфон              | 49990.00 | t        |           1 | 2026-05-26 10:32:25.95435
  2 | Наушники Pro           | Беспроводные наушники с шумодавом |  8990.00 | t        |           1 | 2026-05-26 10:32:25.95435
  3 | Клавиатура Mech        | Механическая клавиатура           |  7490.00 | t        |           1 | 2026-05-26 10:32:25.95435
  4 | Футболка Classic       | Хлопковая футболка                |  1990.00 | t        |           2 | 2026-05-26 10:32:25.95435
  5 | Зимняя куртка          | Тёплая куртка с капюшоном         | 12990.00 | f        |           2 | 2026-05-26 10:32:25.95435
  6 | TypeScript на практике | Книга по TypeScript               |  2500.00 | t        |           3 | 2026-05-26 10:32:25.95435
  7 | PostgreSQL для всех    | Книга по PostgreSQL               |  3200.00 | t        |           3 | 2026-05-26 10:32:25.95435
  8 | Гантели 5 кг           | Пара гантелей                     |  2990.00 | t        |           4 | 2026-05-26 10:32:25.95435
  9 | Коврик для йоги        | Противоскользящий коврик          |  1490.00 | t        |           4 | 2026-05-26 10:32:25.95435
 10 | Планшет Tab8           | 8-дюймовый планшет                | 19990.00 | f        |           1 | 2026-05-26 10:32:25.95435
(10 rows)

          name          |  price   
------------------------+----------
 Смартфон X10           | 49990.00
 Наушники Pro           |  8990.00
 Клавиатура Mech        |  7490.00
 Футболка Classic       |  1990.00
 Зимняя куртка          | 12990.00
 TypeScript на практике |  2500.00
 PostgreSQL для всех    |  3200.00
 Гантели 5 кг           |  2990.00
 Коврик для йоги        |  1490.00
 Планшет Tab8           | 19990.00
(10 rows)

      name       |  price   
-----------------+----------
 Смартфон X10    | 49990.00
 Наушники Pro    |  8990.00
 Клавиатура Mech |  7490.00
 Зимняя куртка   | 12990.00
 Планшет Tab8    | 19990.00
(5 rows)

          name          |  price   | in_stock 
------------------------+----------+----------
 Смартфон X10           | 49990.00 | t
 Наушники Pro           |  8990.00 | t
 Клавиатура Mech        |  7490.00 | t
 PostgreSQL для всех    |  3200.00 | t
 Гантели 5 кг           |  2990.00 | t
 TypeScript на практике |  2500.00 | t
 Футболка Classic       |  1990.00 | t
 Коврик для йоги        |  1490.00 | t
(8 rows)

          name          |  price  
------------------------+---------
 Коврик для йоги        | 1490.00
 Футболка Classic       | 1990.00
 TypeScript на практике | 2500.00
(3 rows)

 total_products 
----------------
             10
(1 row)

 avg_price 
-----------
  11162.00
(1 row)

 cheapest | most_expensive 
----------+----------------
  1490.00 |       49990.00
(1 row)

 total_revenue 
---------------
     109640.00
(1 row)

 category_id | product_count 
-------------+---------------
           3 |             2
           4 |             2
           2 |             2
           1 |             4
(4 rows)

 category_id | product_count | avg_price 
-------------+---------------+-----------
           3 |             2 |   2850.00
           4 |             2 |   2240.00
           2 |             2 |   7490.00
           1 |             4 |  21615.00
(4 rows)

  status   | order_count 
-----------+-------------
 completed |           3
 created   |           2
 shipped   |           1
(3 rows)

 user_id | order_count 
---------+-------------
       2 |           2
       1 |           2
(2 rows)

     name      |  price   
---------------+----------
 Смартфон X10  | 49990.00
 Зимняя куртка | 12990.00
 Планшет Tab8  | 19990.00
(3 rows)

 username |     email      
----------+----------------
 eve      | eve@mail.com
 diana    | diana@mail.com
 bob      | bob@mail.com
 alice    | alice@mail.com
(4 rows)

       name       |  price  
------------------+---------
 Клавиатура Mech  | 7490.00
 Футболка Classic | 1990.00
 Коврик для йоги  | 1490.00
(3 rows)

        name         |  price   | category_id 
---------------------+----------+-------------
 Смартфон X10        | 49990.00 |           1
 Зимняя куртка       | 12990.00 |           2
 PostgreSQL для всех |  3200.00 |           3
 Гантели 5 кг        |  2990.00 |           4
(4 rows)

      product_name      |  price   | category_name 
------------------------+----------+---------------
 Смартфон X10           | 49990.00 | Электроника
 Наушники Pro           |  8990.00 | Электроника
 Клавиатура Mech        |  7490.00 | Электроника
 Футболка Classic       |  1990.00 | Одежда
 Зимняя куртка          | 12990.00 | Одежда
 TypeScript на практике |  2500.00 | Книги
 PostgreSQL для всех    |  3200.00 | Книги
 Гантели 5 кг           |  2990.00 | Спорт
 Коврик для йоги        |  1490.00 | Спорт
 Планшет Tab8           | 19990.00 | Электроника
(10 rows)

 order_id | username |  status   | total_price 
----------+----------+-----------+-------------
        1 | alice    | completed |    58980.00
        3 | bob      | completed |    19990.00
        4 | bob      | shipped   |    12990.00
        2 | alice    | created   |     8990.00
        5 | diana    | completed |     5700.00
        6 | eve      | created   |     2990.00
(6 rows)

practice_db=# /dt
practice_db-# \dt
            List of relations
 Schema |    Name     | Type  |  Owner   
--------+-------------+-------+----------
 public | categories  | table | postgres
 public | order_items | table | postgres
 public | orders      | table | postgres
 public | products    | table | postgres
 public | users       | table | postgres
(5 rows)

practice_db-# select * from categories
practice_db-# select * from categories;
ERROR:  syntax error at or near "/"
LINE 1: /dt
        ^
practice_db=# select * from categories;
 id |    name     |               description               
----+-------------+-----------------------------------------
  1 | Электроника | Гаджеты и устройства
  2 | Одежда      | Верхняя одежда, обувь, аксессуары
  3 | Книги       | Художественная и техническая литература
  4 | Спорт       | Спортивный инвентарь и экипировка
(4 rows)

practice_db=# select name from categories;
    name     
-------------
 Электроника
 Одежда
 Книги
 Спорт
(4 rows)

practice_db=# select * from categories where name like "*a";
ERROR:  column "*a" does not exist
LINE 1: select * from categories where name like "*a";
                                                 ^
practice_db=# select * from categories where name like "%a";
ERROR:  column "%a" does not exist
LINE 1: select * from categories where name like "%a";
                                                 ^
practice_db=# select * from categories where name like '%a';
 id | name | description 
----+------+-------------
(0 rows)

practice_db=# select * from categories where name like '%а';
 id |    name     |            description            
----+-------------+-----------------------------------
  1 | Электроника | Гаджеты и устройства
  2 | Одежда      | Верхняя одежда, обувь, аксессуары
(2 rows)

practice_db=# \dt
            List of relations
 Schema |    Name     | Type  |  Owner   
--------+-------------+-------+----------
 public | categories  | table | postgres
 public | order_items | table | postgres
 public | orders      | table | postgres
 public | products    | table | postgres
 public | users       | table | postgres
(5 rows)

practice_db=# select * from products
practice_db-# select * from products;
ERROR:  syntax error at or near "select"
LINE 2: select * from products;
        ^
practice_db=# select * from products
select * from products;
ERROR:  syntax error at or near "select"
LINE 2: select * from products;
        ^
practice_db=# select * from products;
 id |          name          |            description            |  price   | in_stock | category_id |        created_at         
----+------------------------+-----------------------------------+----------+----------+-------------+---------------------------
  1 | Смартфон X10           | Флагманский смартфон              | 49990.00 | t        |           1 | 2026-05-26 10:32:25.95435
  2 | Наушники Pro           | Беспроводные наушники с шумодавом |  8990.00 | t        |           1 | 2026-05-26 10:32:25.95435
  3 | Клавиатура Mech        | Механическая клавиатура           |  7490.00 | t        |           1 | 2026-05-26 10:32:25.95435
  4 | Футболка Classic       | Хлопковая футболка                |  1990.00 | t        |           2 | 2026-05-26 10:32:25.95435
  5 | Зимняя куртка          | Тёплая куртка с капюшоном         | 12990.00 | f        |           2 | 2026-05-26 10:32:25.95435
  6 | TypeScript на практике | Книга по TypeScript               |  2500.00 | t        |           3 | 2026-05-26 10:32:25.95435
  7 | PostgreSQL для всех    | Книга по PostgreSQL               |  3200.00 | t        |           3 | 2026-05-26 10:32:25.95435
  8 | Гантели 5 кг           | Пара гантелей                     |  2990.00 | t        |           4 | 2026-05-26 10:32:25.95435
  9 | Коврик для йоги        | Противоскользящий коврик          |  1490.00 | t        |           4 | 2026-05-26 10:32:25.95435
 10 | Планшет Tab8           | 8-дюймовый планшет                | 19990.00 | f        |           1 | 2026-05-26 10:32:25.95435
(10 rows)

practice_db=# select * from products where price > 10000;
 id |     name      |        description        |  price   | in_stock | category_id |        created_at         
----+---------------+---------------------------+----------+----------+-------------+---------------------------
  1 | Смартфон X10  | Флагманский смартфон      | 49990.00 | t        |           1 | 2026-05-26 10:32:25.95435
  5 | Зимняя куртка | Тёплая куртка с капюшоном | 12990.00 | f        |           2 | 2026-05-26 10:32:25.95435
 10 | Планшет Tab8  | 8-дюймовый планшет        | 19990.00 | f        |           1 | 2026-05-26 10:32:25.95435
(3 rows)

practice_db=# select * from products order by price desc;
 id |          name          |            description            |  price   | in_stock | category_id |        created_at         
----+------------------------+-----------------------------------+----------+----------+-------------+---------------------------
  1 | Смартфон X10           | Флагманский смартфон              | 49990.00 | t        |           1 | 2026-05-26 10:32:25.95435
 10 | Планшет Tab8           | 8-дюймовый планшет                | 19990.00 | f        |           1 | 2026-05-26 10:32:25.95435
  5 | Зимняя куртка          | Тёплая куртка с капюшоном         | 12990.00 | f        |           2 | 2026-05-26 10:32:25.95435
  2 | Наушники Pro           | Беспроводные наушники с шумодавом |  8990.00 | t        |           1 | 2026-05-26 10:32:25.95435
  3 | Клавиатура Mech        | Механическая клавиатура           |  7490.00 | t        |           1 | 2026-05-26 10:32:25.95435
  7 | PostgreSQL для всех    | Книга по PostgreSQL               |  3200.00 | t        |           3 | 2026-05-26 10:32:25.95435
  8 | Гантели 5 кг           | Пара гантелей                     |  2990.00 | t        |           4 | 2026-05-26 10:32:25.95435
  6 | TypeScript на практике | Книга по TypeScript               |  2500.00 | t        |           3 | 2026-05-26 10:32:25.95435
  4 | Футболка Classic       | Хлопковая футболка                |  1990.00 | t        |           2 | 2026-05-26 10:32:25.95435
  9 | Коврик для йоги        | Противоскользящий коврик          |  1490.00 | t        |           4 | 2026-05-26 10:32:25.95435
(10 rows)

practice_db=# select * from products limit 5;
 id |       name       |            description            |  price   | in_stock | category_id |        created_at         
----+------------------+-----------------------------------+----------+----------+-------------+---------------------------
  1 | Смартфон X10     | Флагманский смартфон              | 49990.00 | t        |           1 | 2026-05-26 10:32:25.95435
  2 | Наушники Pro     | Беспроводные наушники с шумодавом |  8990.00 | t        |           1 | 2026-05-26 10:32:25.95435
  3 | Клавиатура Mech  | Механическая клавиатура           |  7490.00 | t        |           1 | 2026-05-26 10:32:25.95435
  4 | Футболка Classic | Хлопковая футболка                |  1990.00 | t        |           2 | 2026-05-26 10:32:25.95435
  5 | Зимняя куртка    | Тёплая куртка с капюшоном         | 12990.00 | f        |           2 | 2026-05-26 10:32:25.95435
(5 rows)

practice_db=#
```