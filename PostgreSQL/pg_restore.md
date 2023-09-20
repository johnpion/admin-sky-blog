# Postgresql. Restore

If you have powerfull server and don't want wait for the infinity with 1 stream you can use:

```shell
pg_restore -j 16 -F d -v dbname_20221205_2151.backup -d dbname
```
