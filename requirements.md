## Generic

### Small Memory Footprint

`req~small-memory-footprint~1`

Keep the required memory footprint for the UDFs low.

Rationale:

By default, an Exasol cluster node reserves ~ 4 GB RAM for UDFs.
It's the minimum of the DB-RAM (2 GB on a default docker-db, much higher in clusters), a quota for RAM for all processes (~ 32 GB per node) on a node and a quote for the RAM for one SQL session (~ 4 GB per node).
That's independent of how much RAM is available on that node and on how many CPUs it has.
That RAM quota is shared by all UDF instances in a query.
Onn the other hand, for a quick loading parallelization is usually crucial.
So in order to be able to run many UDFs at the same time we need to reduce the memory footprint.

### Custom Loader

`req~custom-loader~1`

Exasol can't load the required file types using the IMPORT statement. So we need to implement a custom mechanism for retrieving these files.

Needs: dsn

### Unified Connection Definition

`req~unified-connection-def~1`

The Virtual Schemas use connection parameters specified in the Exasol [connection-parameter-specification](https://github.com/exasol/connection-parameter-specification/).

Rationale:

Customers should be able to use the same connection for different Exasol Integration products. Different syntaxes for the same thing are confusing.

### Filter Pushdown

`req~filter-pushdown~1`

Each datasource implements pushing down predicates to the data source.

Rationale:

Each datasource can have a different API. There is no generic methos for sending the predicates.

Covers:

* `feat~filer-pushdown~1`

Needs: dsn per dialect

### Query Splitting

`req~query-splitting~1`

The Virtual Schema adapter can transform the WHERE clause of a query into a push-down-selection and a post-selection. The push-down selection only contains predicates that can be pushed down to the data source. The post selection is applied after the transfer in the Exasol DB.

The result mus always be exactly the same as if we load the whole table and apply the WHERE clause in the Exasol DB.

Rationale:

It depends on the datasource which predicates we can push. Often a query will contain predicates that can be pushed and others that can't.

Covers:

* `feat~filer-pushdown~1`

Needs: dsn

### Source Reference Column

`req~source-reference-column~1`

This virtual schema offers a `SOURCE_REFERENCE` column that contains the name of the file or table the data originates from.

Rationale:

Sometimes that also the filename can contain important information. For example the date of a log file. That's of special relevance when selecting the source via a wildcard and by that the table in Exasol contains data from multiple files.

By allowing filters on the `SOURCE_REFERENCE` columns we can support a simple filter-pushdown for data sources that can't filter the data properly.

## Files Virtual Schemas

### Orthogonal Extension Mechanism for file types and storage backends

* `req~orthogonal-file-type-and-backend-extension-mechanism~1`

The Virtual Schema should support all file types on all data sources.
The extension mechanism should be independent. The storage backend implementation should not need to know about the file types and vice versa.

This is important, since it keeps the implementation effort low. If we have `n` file types and `m` storage backends we only need to implement `n + m` plugins.
If it was coupled, it were `n * m`.