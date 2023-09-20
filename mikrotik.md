# Mikrotik

## Backup
```
/export file=mikrotik_v01_YYYY-MM-dd
/export file=mikrotik_v01_YYYY-MM-dd_verbose  verbose
/system backup save name=mikrotik_v01_YYYY-MM-dd password=PASSWORD
```

## RouterOS v7.X
RouterOS v7.X uses new syntax with `/`:
```
/ip/address/print
```

RouterOS v6.X
```
/ip address print
```
