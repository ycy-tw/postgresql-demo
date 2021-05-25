# PostgreSQL-Demo

### psql command
```SQL
set PGUSER=postgres
\! cls   -- clean screen
\! clear     -- clean screen
\l -- show all databases
\l+ -- show all databases and descriptions
\c database_name -- switch env under database_name
\dt -- show all tables
\dt+ -- show all tables and descriptions
```

### CREATE TABLE

```SQL
CREATE TABLE price (
	symbol 	integer PRIMARY KEY,
	name	character(255),
	shares	INT,
	volume	INT,
	open	real,
	high	real,
	low	real,
	close	real,
	ret	real
);

CREATE TABLE basic_info (
	symbol 	integer PRIMARY KEY,
	name	character(255),
	industry    text,
	listed_date date
);
```

### Insert data from .csv
```SQL
\copy price FROM 'C:/Users/user/price.csv' DELIMITER ',' CSV HEADER ;
```


### Insert data.
```SQL
INSERT INTO price (symbol, name, shares, volume, open, high, low, close, ret)
VALUES (9999, '測試', 20000, 150233121, 20.5, 21, 20.1, 20.8, 0.014);

INSERT INTO basic_info (symbol, name, industry, listed_date)
VALUES (9999, '測試', '測試產業', '2020-01-03');
```

### Update column values.
```sql=
UPDATE price
SET ret = ret * 100;
```


### Get stocks which close price under 10. 
```sql=
SELECT * FROM price
WHERE price.close < 10;
```

### Get each stock's industry. Combined two tables with symbols.
```sql=
SELECT price.ret, basic_info.industry
FROM price, basic_info
WHERE price.symbol = basic_info.symbol;
```

### Get industry average daily return.
```sql=
SELECT cast(AVG(ret) as decimal(10,3)), industry FROM (
	SELECT price.ret, basic_info.industry
	FROM price, basic_info
	WHERE price.symbol = basic_info.symbol
) AS ret_industry
GROUP BY industry;
```

### Sort by average return.
#### 
```sql=
-- ASC  <-- e.g. 10, 9, ..., 2 ,1
-- DESC 

SELECT cast(AVG(ret) as decimal(10,3)), industry FROM (
	SELECT price.ret, basic_info.industry
	FROM price, basic_info
	WHERE price.symbol = basic_info.symbol
) AS ret_industry
GROUP BY industry
ORDER BY AVG(ret) DESC;
```

### Delete all values in table.
```sql=
DELETE FROM price;
DELETE FROM basic_info;
```

### Or just Drop it.

```sql=
DROP TABLE price, basic_info;
```

---

# PostgreSQL Official Document Tutorial

```sql=
createdb dbname
dropdb dbname

set PGUSER=postgres

SELECT version();
```

White space (i.e., spaces, tabs, and newlines) can be used freely in SQL commands.
Two dashes (“--”) introduce comments. Whatever follows them is ignored up to the end of the line


# Create
```sql=
CREATE TABLE tablename (
column1 dataTypeOfColumn1,   -- Description of column1
column2 dataTypeOfColumn2,
column3 dataTypeOfColumn3,
);
```



# Create
INSERT INTO tablename VALUES(valueOfColumn1, valueOfColumn2, valueOfColumn3);
```sql=
INSERT INTO weather (column1, column2, column3)
VALUES (valueOfColumn1, valueOfColumn2, valueOfColumn3);
```

You can list the columns in a different order if you wish or even omit some columns,
```sql=
INSERT INTO weather (column3, column2)
VALUES (valueOfColumn3, valueOfColumn2);
```

# Read
```sql=
SELECT * FROM tablename;
SELECT column1, column2, column3 FROM tablename;
```

```sql=
--- Notice how the AS clause is used to relabel the output column. (The AS clause is optional.)
SELECT column1, (column2 + column3)/2 AS avg_column_2_3 FROM tablename;
```

```sql=
SELECT * FROM tablename
WHERE column1 = 'xxx' AND column2 > 0.5;
```

```sql=
SELECT * FROM tablename
ORDER BY column1;
```

You can request that duplicate rows be removed from the result of a query
```sql=
SELECT DISTINCT column1
FROM tablename;
```

join two tables
```sql=
SELECT col1_table1 col2_table1 col1_table2  col2_table2
FROM tablename1, tablename2;
WHERE col1_tablename1 = col3_tablename2;
```

If there were duplicate column names in the two tables you'd need to qualify the column names to show which one you meant.
```sql=
SELECT col1_table1.tablename1 col2_table1.tablename1 col1_table2.tablename2  col2_table2.tablename2
FROM tablename1, tablename2;
WHERE col1_tablename1.tablename1 = col3_tablename2.tablename2;
```

If no matching row is found we want some “empty values” to be substituted for the tablename1 table's columns. This kind of query is called an outer join.

```sql=
SELECT *
FROM tablename1 LEFT OUTER JOIN tablename2 ON (col1_tablename1.tablename1 =
col3_tablename2.tablename2);
```

This query is called a left outer join because the table mentioned on the left of the join operator will have
each of its rows in the output at least once, whereas the table on the right will only have those rows output
that match some row of the left table. When outputting a left-table row for which there is no right-table
match, empty (null) values are substituted for the right-table columns.


```sql=
SELECT max(column1) FROM tablename1;

SELECT column1 FROM tablename1
WHERE column2 = (SELECT max(column2) FROM tablename1);
```

```sql=
SELECT column1, max(column2)
FROM tablename1
GROUP BY column1;
```

The fundamental difference between WHERE and HAVING is this: WHERE selects input rows before groups
and aggregates are computed (thus, it controls which rows go into the aggregate computation), whereas
HAVING selects group rows after groups and aggregates are computed.

```sql=
SELECT column1, max(column2)
FROM tablename1
GROUP BY column1;
HAVING max(column2) < 40;
```

In the previous example, we can apply the city name restriction in WHERE, since it needs no aggregate.
This is more efficient than adding the restriction to HAVING, because we avoid doing the grouping and
aggregate calculations for all rows that fail the WHERE check.


# Update
```sql=
UPDATE tablename1
SET column2 = column2 - 2,
WHERE column1 > 'XXXX-XX-XX';
```

# Delete

delete row
```sql=
DELETE FROM tablename1 WHERE column1 = 'XXXX-XX-XX';
```

delete values in table.
```sql=
DELETE FROM tablename1;
```

delete table.
```sql=
DROP TABLE tablename;
```
