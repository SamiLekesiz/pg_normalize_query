# pg_normalize_query
PostgreSQL extension to normalize a SQL query similar to [pg_stat_statements](https://www.postgresql.org/docs/current/pgstatstatements.html).

This code was extracted from [libpg_query](https://github.com/lfittl/libpg_query/blob/10-latest/src/pg_query_normalize.c) and transformed into a PostgreSQL extension.

## Installation

```sh
$ git clone https://github.com/fabriziomello/pg_normalize_query.git
$ cd pg_normalize_query
# Make sure your path includes the bin directory that contains the correct `pg_config`
$ PATH=/path/to/pg/bin:$PATH
$ USE_PGXS=1 make
$ USE_PGXS=1 make install
```

## Tests

```sh
$ USE_PGXS=1 make installcheck
```

## Examples

```
fabrizio=# CREATE EXTENSION pg_normalize_query;
CREATE EXTENSION
fabrizio=# \dx
                       List of installed extensions
        Name        | Version |   Schema   |         Description          
--------------------+---------+------------+------------------------------
 pg_normalize_query | 1.0     | public     | Normalize SQL Query
 plpgsql            | 1.0     | pg_catalog | PL/pgSQL procedural language
(2 rows)

fabrizio=# SELECT pg_normalize_query($$SELECT * FROM pg_proc WHERE proname = 'pg_normalize_query'$$);
            pg_normalize_query            
------------------------------------------
 SELECT * FROM pg_proc WHERE proname = $1
(1 row)

fabrizio=# SELECT pg_normalize_query($$SELECT oid, relname, relkind FROM pg_class WHERE relkind IN ('r', 'i') LIMIT 10$$);
                              pg_normalize_query                               
-------------------------------------------------------------------------------
 SELECT oid, relname, relkind FROM pg_class WHERE relkind IN ($1, $2) LIMIT $3
(1 row)
```
Please feel free to [open a PR](https://github.com/fabriziomello/pg_normalize_query/pull/new/master).

## Authors

- [Fabrízio de Royes Mello](mailto:fabriziomello@gmail.com)