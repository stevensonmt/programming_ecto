# Chapter 1

**MODULES**
* `Repo` -- database proxy
* `Query` -- API for retrieving data from DB
* `Schema` -- map database to application code
* `Changeset` -- update/manipulate data(?)
* `Multi` -- coordinate multiple separate databases
* `Migration` -- keep DB in sync with application as structure of DB evolves

**Ecto Architecture**
* `ecto` -- core data manipulation modules that can be used independent of DB
* `ecto_sql` -- relational DB communication modules

In application's custom repo module defining `init/2` allows getting DB URL from system env to avoid hardcoding username/passwd in source.
DB URL format is "ecto://USERNAME:PASSWORD@HOSTNAME/DATABASE_NAME" such as "ecto://matt:@localhost/music_db". Set this as ENV var such as `MUSIC_DB_URL` and retrieve at `*.Repo.init` call with `System.get_env("MUSIC_DB_URL")`.

Pin operator (`^`) protects against SQL injection attacks by parameterizing values.
Using `like/2` with user generated queries can allow LIKE injections.

Query module can be extended with custom macros that are imported into module. Use case is for queries that will use `fragment` regularly.

Union queries must be wrapped in subquery to use order_by on whole query or use `fragment` in `order_by` parameter of component query.

```
    supplier_query = from s in Supplier, select: s.city
    union_all_query = from c in Customer, select: c.city, union_all: ^supplier_query
    from s in subquery(union_all_query), order_by: s.city

    customer_query = Customer |> select([c], c.city) |> order_by(fragment("city"))
    supplier_query = Supplier |> select([s], s.city)
    union_all(customer_query, ^supplier_query)
```

`union` vs `union_all`: former ensures no duplicates but can be less efficient due to filtering step
Same for `intersect`/`intersect_all` and `except`/`except_all`
