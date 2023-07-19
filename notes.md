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
