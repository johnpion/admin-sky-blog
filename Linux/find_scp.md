# find + scp
How to copy only files from the past few days?

```shell
find $BACKUP_DIR -name "*.xva" -type f -mtime -$BACKUP_AGE -exec \
scp '{}' $BACKUP_REMOTE_SERVER:$BACKUP_REMOTE_DIR ;
```
