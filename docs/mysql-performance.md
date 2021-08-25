---

template:      article
reviewed:      2021-08-25
title:         MySQL performance tips
naviTitle:     MySQL performance tips
excerpt:       Some tips for a better MySQL performance.
lead:          'This article guides to identifying common performance issues with your MySQL database on fortrabbit.'
group:         tips
stack:         all
dontList:      true

---

## The problem

Performance problems here on fortrabbit - and surely on other hosting platforms as well - are often caused by not making proper use of the available resources. Specifically this applies to misusage of the MySQL database resources provided. Those not only consist of an available storage size, but also computing power - think CPU cycles. Also mind that most of our database plans are running on shared resources.

Most 504 errors and slow PHP response times are related to slow MySQL queries.

It's you job as the responsible developer to make sure no insane MySQL queries are executed with your App.

When using a framework or CMS you might not write MySQL queries directly. Still you need to take that the requests to the database are executed without waste.

## Do's

### Consider real usage scenarios

Most commonly database problems are not becoming visible in development when there are just a couple of rows in the tables. Also consider that a local machine might have more memory and CPU available, than the App here hosted on fortrabbit. In addition, consider that there might be more parallel requests to the App ongoing.

### Use profiling

There are a couple of tools you cna use to identify and debug slow queries:

#### Framework and CMS plugins

* Laravel Debugbar
* Yii Debug Toolbar for Craft CMS

#### External services

The following external services are available as extensions with fortrabbit:

* New Relic - [see integration guide](/new-relic)
* Blackfire - [see integration guide](/blackfire)

### Split write-tables & read-tables

Try to use separate write-intensive and read-intensive tables. Whenever you write on a table, you invalidate the query cache. For instance: if you have a read-intensive user table, and want to log their latest activity, use an additional table for this.

### Use Smart indexing

**First off: Use indexes.** But which? Look at your queries. MySQL provides [EXPLAIN](https://dev.mysql.com/doc/refman/8.0/en/explain.html). EXPLAIN shows you which columns and which of their indices (if any) are used. It also points out if inefficient strategies, such as "[temporary tables](https://dev.mysql.com/doc/refman/5.7/en/internal-temporary-tables.html)" or "[filesort](https://www.percona.com/blog/2009/03/05/what-does-using-filesort-mean-in-mysql/)" are applied. 

So: Log your queries, run them through EXPLAIN and build your (multi-column) indexes based on the EXPLAIN results. Most importantly: repeat this from time to time, because things change and different data-size proportions might benefit from different indexes.

Also see [this article about indexing in MySQL](https://opensource.com/article/17/5/speed-your-mysql-queries-300-times) by Anna Filina.

## Don'ts

### Avoid JOINs

JOINs are easily used with modern ORMs and look like a good idea at first glance (and in certain situations they are!). However, when your App grows, they are many times the reason why your database performance decreases hugely. Why? Because there is a major difference between a join between 100 x 100 rows and 10,000 x 100,000 rows: the former fits in the memory, the latter not.

### Avoid full table scans

The output from EXPLAIN shows ALL in the type column when MySQL uses a full table scan to resolve a query. This usually happens under the following conditions:

- tables with few rows (index lookup not worth it)
- there is no ON or WHERE conditions for indexed columns
- constant values in columns spanning large portion of rows
- low cardinality index (many similar values in column)

For small tables, a table scan often is appropriate and the performance impact is negligible. For large tables, try the following techniques to avoid having the optimizer incorrectly choose a table scan:

- Use ANALYZE TABLE tbl_name to update the key distributions for the scanned table. See Section 13.7.3.1, "ANALYZE TABLE" Statement.
- Use FORCE INDEX for the scanned table to tell MySQL that table scans are very expensive compared to using the given index:

```sql
SELECT * FROM t1, t2 FORCE INDEX (index_for_column)
WHERE t1.col_name=t2.col_name;
See Section 8.9.4, "Index Hints".
```

#### Links to full table scans

- [Wikipedia definition](https://en.wikipedia.org/wiki/Full_table_scan)
- [MySQL documentation](https://dev.mysql.com/doc/refman/8.0/en/table-scan-avoidance.html)
- [Stack Overflow: How can I avoid a full table scan on this mysql query?](https://stackoverflow.com/questions/14106770/how-can-i-avoid-a-full-table-scan-on-this-mysql-query/14106862)
- [Stack Overflow:Mysql Explain Query with type "ALL" when an index is used](https://stackoverflow.com/questions/20700698/mysql-explain-query-with-type-all-when-an-index-is-used)