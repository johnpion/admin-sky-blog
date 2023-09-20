# sort by column

Sometimes you might need to sort a list, not by the first column, but by any other column:

```shell
sort -t : -k 3
```

This command will sort the list based on the third column. The "-t" flag specifies ":" as the field separator, and "-k" indicates which column to sort by.
