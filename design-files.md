# Design for the Virtual Schema for Document Files

## Filter Push-Down

`dsn~filet-pushdown~1`

For the files Virtual Schema we can not push selection on the data since the backends are just file stores. They don't support APIs for filtering based on the file content.

However, often the filename contains some information that can be useful for selecting the correct dataset to load. So the files virtual schemas support `=` and `LIKE` predicates on the `SOURCE_REFERENCE` column.

Covers:

* `req~filter-pushdown~1`