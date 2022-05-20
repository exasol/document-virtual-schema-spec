# Features

**Purpose of this project:** Users can integrate No-SQL data sources into their Exasol database as a Virtual Schema

### Mapping to Fixed Data Types

The Virtual Schema adapter converts the semi-structured document data into a relational data format.

That has two main challenges:

#### Different data types in each row

The virtual schema adapter dynamically converts the source data to the target data type.

Rationale:

In semi-structured data, each document can have a different structure. It's also possible that a property has different types between different documents.

#### Handle nested lists

The virtual schema adapter can convert nested lists from documents so that they can be processed in Exasol.

Rationale:

The relational model of Exasol does not support nested lists.

###  Filter Push-Down

`feat~filer-pushdown~1`

The Virtual Schema adapter pushes as many predicates from the WHERE clause as possible down to the data source.

Needs: genericdsn, dialectdsn

### Support for Different Data Sources

We offer Virtual Schema adapter for accessing the following types of No-SQL data:

* DynamoDB <!-- `feat~dynamodb~1` -->
* Files
    * with file types:
        * JSON
        * JSON-lines
        * Parquet
    * with storage backends
        * from AWS S3
        * from Google Cloud Storage
        * from BucketFS

### Fast Loading for Different Load Patterns

The Virtual Schema performs well for the following load patterns:

* Many small files
* One / few big files
* One / few small files
