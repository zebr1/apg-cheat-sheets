# PySpark Cheat Sheet

## Initialization

```python
# Import PySpark
from pyspark.sql import SparkSession
from pyspark.sql.functions import *
from pyspark.sql.types import *

# Create Spark session
spark = SparkSession.builder \
    .appName("MyApp") \
    .config("spark.some.config.option", "value") \
    .getOrCreate()

# Get Spark context
sc = spark.sparkContext

# Stop Spark session
spark.stop()
```

## Creating DataFrames

```python
# From list of tuples
data = [("Alice", 25), ("Bob", 30), ("Charlie", 35)]
df = spark.createDataFrame(data, ["name", "age"])

# From list of dictionaries
data = [{"name": "Alice", "age": 25}, {"name": "Bob", "age": 30}]
df = spark.createDataFrame(data)

# From pandas DataFrame
import pandas as pd
pandas_df = pd.DataFrame({"name": ["Alice", "Bob"], "age": [25, 30]})
df = spark.createDataFrame(pandas_df)

# Read from CSV
df = spark.read.csv("path/to/file.csv", header=True, inferSchema=True)

# Read from JSON
df = spark.read.json("path/to/file.json")

# Read from Parquet
df = spark.read.parquet("path/to/file.parquet")

# Read from Delta table
df = spark.read.format("delta").load("path/to/delta/table")

# Read with options
df = spark.read \
    .option("header", "true") \
    .option("delimiter", ",") \
    .option("inferSchema", "true") \
    .csv("path/to/file.csv")
```

## Basic DataFrame Operations

```python
# Show data
df.show()
df.show(5)              # Show first 5 rows
df.show(truncate=False) # Show without truncating

# Print schema
df.printSchema()

# Get column names
df.columns

# Get number of rows and columns
df.count()
len(df.columns)

# Describe statistics
df.describe().show()
df.describe("age", "salary").show()

# Select columns
df.select("name", "age").show()
df.select(col("name"), col("age")).show()
df.select(df.name, df.age).show()

# Select all columns
df.select("*").show()

# Select with expressions
df.select(col("name"), (col("age") + 1).alias("next_age")).show()

# Drop columns
df.drop("column_name").show()
df.drop("col1", "col2").show()

# Rename columns
df.withColumnRenamed("old_name", "new_name").show()

# Add new column
df.withColumn("new_col", col("age") * 2).show()

# Cast column type
df.withColumn("age", col("age").cast("double")).show()
```

## Filtering and Conditional Operations

```python
# Filter rows
df.filter(col("age") > 30).show()
df.filter(df.age > 30).show()
df.filter("age > 30").show()
df.where(col("age") > 30).show()

# Multiple conditions
df.filter((col("age") > 25) & (col("age") < 35)).show()
df.filter((col("age") > 35) | (col("name") == "Alice")).show()

# Filter with NOT
df.filter(~(col("age") > 30)).show()

# Filter NULL values
df.filter(col("age").isNull()).show()
df.filter(col("age").isNotNull()).show()

# Filter with IN
df.filter(col("name").isin(["Alice", "Bob"])).show()

# Filter with LIKE
df.filter(col("name").like("A%")).show()
df.filter(col("name").startswith("A")).show()
df.filter(col("name").endswith("e")).show()
df.filter(col("name").contains("li")).show()
```

## Sorting

```python
# Sort ascending
df.sort("age").show()
df.orderBy("age").show()
df.sort(col("age")).show()

# Sort descending
df.sort(col("age").desc()).show()
df.orderBy(col("age").desc()).show()

# Sort by multiple columns
df.sort("age", "name").show()
df.orderBy(col("age").desc(), col("name").asc()).show()
```

## Aggregations

```python
# Basic aggregations
df.groupBy("department").count().show()
df.groupBy("department").sum("salary").show()
df.groupBy("department").avg("salary").show()
df.groupBy("department").max("salary").show()
df.groupBy("department").min("salary").show()

# Multiple aggregations
df.groupBy("department").agg(
    count("*").alias("count"),
    sum("salary").alias("total_salary"),
    avg("salary").alias("avg_salary"),
    max("salary").alias("max_salary"),
    min("salary").alias("min_salary")
).show()

# Aggregation without grouping
df.agg(
    count("*").alias("total_count"),
    sum("salary").alias("total_salary")
).show()

# Distinct count
df.select(countDistinct("name")).show()
df.groupBy("department").agg(countDistinct("name").alias("unique_names")).show()
```

## Joins

```python
# Inner join
df1.join(df2, df1.id == df2.id, "inner").show()
df1.join(df2, "id", "inner").show()  # When column names match

# Left join
df1.join(df2, df1.id == df2.id, "left").show()
df1.join(df2, "id", "left").show()

# Right join
df1.join(df2, df1.id == df2.id, "right").show()

# Full outer join
df1.join(df2, df1.id == df2.id, "outer").show()

# Left semi join (returns rows from left where match exists)
df1.join(df2, df1.id == df2.id, "leftsemi").show()

# Left anti join (returns rows from left where no match exists)
df1.join(df2, df1.id == df2.id, "leftanti").show()

# Multiple join conditions
df1.join(df2, (df1.id == df2.id) & (df1.date == df2.date), "inner").show()

# Cross join (Cartesian product)
df1.crossJoin(df2).show()
```

## Window Functions

```python
from pyspark.sql.window import Window

# Define window specification
windowSpec = Window.partitionBy("department").orderBy("salary")

# Row number
df.withColumn("row_num", row_number().over(windowSpec)).show()

# Rank and dense rank
df.withColumn("rank", rank().over(windowSpec)).show()
df.withColumn("dense_rank", dense_rank().over(windowSpec)).show()

# Lead and lag
df.withColumn("next_salary", lead("salary", 1).over(windowSpec)).show()
df.withColumn("prev_salary", lag("salary", 1).over(windowSpec)).show()

# Cumulative sum
df.withColumn("cumsum", sum("salary").over(windowSpec)).show()

# Moving average
windowSpec = Window.partitionBy("department").orderBy("date").rowsBetween(-2, 0)
df.withColumn("moving_avg", avg("value").over(windowSpec)).show()
```

## String Functions

```python
# Convert case
df.select(upper(col("name"))).show()
df.select(lower(col("name"))).show()
df.select(initcap(col("name"))).show()

# Substring
df.select(substring(col("name"), 1, 3)).show()

# Concatenation
df.select(concat(col("first_name"), lit(" "), col("last_name"))).show()
df.select(concat_ws("-", col("year"), col("month"), col("day"))).show()

# Trim
df.select(trim(col("name"))).show()
df.select(ltrim(col("name"))).show()
df.select(rtrim(col("name"))).show()

# Replace
df.select(regexp_replace(col("text"), "pattern", "replacement")).show()

# Split
df.select(split(col("text"), ",")).show()

# Length
df.select(length(col("name"))).show()
```

## Date and Time Functions

```python
# Current date and time
df.select(current_date(), current_timestamp()).show()

# Extract date parts
df.select(
    year(col("date")),
    month(col("date")),
    dayofmonth(col("date")),
    hour(col("timestamp")),
    minute(col("timestamp"))
).show()

# Date arithmetic
df.select(date_add(col("date"), 7)).show()
df.select(date_sub(col("date"), 7)).show()
df.select(datediff(col("end_date"), col("start_date"))).show()
df.select(months_between(col("end_date"), col("start_date"))).show()

# Date formatting
df.select(date_format(col("date"), "yyyy-MM-dd")).show()

# String to date
df.select(to_date(col("date_string"), "yyyy-MM-dd")).show()
df.select(to_timestamp(col("timestamp_string"), "yyyy-MM-dd HH:mm:ss")).show()

# Unix timestamp
df.select(unix_timestamp(col("timestamp"))).show()
df.select(from_unixtime(col("unix_time"))).show()
```

## NULL Handling

```python
# Drop rows with NULL
df.dropna().show()                    # Drop if any column has NULL
df.dropna(how="all").show()           # Drop if all columns are NULL
df.dropna(subset=["age", "name"]).show()  # Drop if specific columns have NULL

# Fill NULL values
df.fillna(0).show()                   # Fill all numeric NULLs with 0
df.fillna({"age": 0, "name": "Unknown"}).show()  # Fill specific columns

# Replace values
df.replace(["old_value"], ["new_value"]).show()
df.replace(["old1", "old2"], ["new1", "new2"]).show()

# Coalesce (return first non-null value)
df.select(coalesce(col("col1"), col("col2"), lit("default"))).show()
```

## Conditional Expressions

```python
# When-otherwise (like CASE WHEN)
df.withColumn(
    "category",
    when(col("age") < 18, "minor")
    .when((col("age") >= 18) & (col("age") < 65), "adult")
    .otherwise("senior")
).show()

# If-else using expr
df.withColumn(
    "status",
    expr("CASE WHEN age >= 18 THEN 'adult' ELSE 'minor' END")
).show()
```

## Union and Set Operations

```python
# Union (includes duplicates)
df1.union(df2).show()

# Union (removes duplicates)
df1.union(df2).distinct().show()

# Intersection
df1.intersect(df2).show()

# Except (in df1 but not in df2)
df1.subtract(df2).show()

# Remove duplicates
df.distinct().show()
df.dropDuplicates().show()
df.dropDuplicates(["column1", "column2"]).show()
```

## Pivot and Unpivot

```python
# Pivot
df.groupBy("year").pivot("month").sum("sales").show()
df.groupBy("year").pivot("month", ["Jan", "Feb", "Mar"]).sum("sales").show()

# Unpivot (using stack)
df.select("id", expr("stack(3, 'col1', col1, 'col2', col2, 'col3', col3) as (key, value)")).show()
```

## User Defined Functions (UDF)

```python
from pyspark.sql.functions import udf

# Define UDF
def square(x):
    return x * x

square_udf = udf(square, IntegerType())

# Use UDF
df.withColumn("squared", square_udf(col("value"))).show()

# UDF with decorator
@udf(returnType=StringType())
def to_uppercase(s):
    return s.upper()

df.withColumn("upper_name", to_uppercase(col("name"))).show()

# Pandas UDF (more efficient)
from pyspark.sql.functions import pandas_udf
import pandas as pd

@pandas_udf(IntegerType())
def pandas_square(s: pd.Series) -> pd.Series:
    return s * s

df.withColumn("squared", pandas_square(col("value"))).show()
```

## Writing Data

```python
# Write to CSV
df.write.csv("path/to/output", header=True, mode="overwrite")

# Write to JSON
df.write.json("path/to/output", mode="overwrite")

# Write to Parquet
df.write.parquet("path/to/output", mode="overwrite")

# Write to Delta
df.write.format("delta").mode("overwrite").save("path/to/delta")

# Partition by column
df.write.partitionBy("year", "month").parquet("path/to/output")

# Write modes
df.write.mode("overwrite").parquet("path")  # Overwrite existing data
df.write.mode("append").parquet("path")     # Append to existing data
df.write.mode("ignore").parquet("path")     # Ignore if exists
df.write.mode("error").parquet("path")      # Error if exists (default)

# Save as table
df.write.saveAsTable("database.table_name")

# Write with coalesce (control number of files)
df.coalesce(1).write.csv("path/to/output")

# Write with repartition
df.repartition(10).write.parquet("path/to/output")
```

## SQL Queries

```python
# Register DataFrame as temp view
df.createOrReplaceTempView("people")

# Run SQL query
result = spark.sql("SELECT name, age FROM people WHERE age > 25")
result.show()

# Register as global temp view (accessible across sessions)
df.createOrReplaceGlobalTempView("people")
spark.sql("SELECT * FROM global_temp.people").show()
```

## Performance Optimization

```python
# Cache DataFrame
df.cache()
df.persist()

# Unpersist
df.unpersist()

# Repartition
df.repartition(10).show()                     # Number of partitions
df.repartition("column").show()               # By column
df.repartition(10, "column").show()           # Both

# Coalesce (reduce partitions without shuffle)
df.coalesce(1).show()

# Broadcast join (for small tables)
from pyspark.sql.functions import broadcast
df1.join(broadcast(df2), "id").show()

# Explain query plan
df.explain()
df.explain(True)  # Extended explanation
```

## RDD Operations

```python
# Create RDD
rdd = sc.parallelize([1, 2, 3, 4, 5])

# Map
rdd.map(lambda x: x * 2).collect()

# Filter
rdd.filter(lambda x: x > 2).collect()

# Reduce
rdd.reduce(lambda x, y: x + y)

# FlatMap
rdd.flatMap(lambda x: [x, x * 2]).collect()

# Convert RDD to DataFrame
rdd.toDF(["column_name"])

# Convert DataFrame to RDD
df.rdd.collect()
```

## Common Patterns

```python
# Count by category
df.groupBy("category").count().orderBy("count", ascending=False).show()

# Top N per group
windowSpec = Window.partitionBy("department").orderBy(col("salary").desc())
df.withColumn("rank", rank().over(windowSpec)).filter(col("rank") <= 3).show()

# Running total
windowSpec = Window.partitionBy("account").orderBy("date").rowsBetween(Window.unboundedPreceding, Window.currentRow)
df.withColumn("running_total", sum("amount").over(windowSpec)).show()

# Percentage of total
total = df.agg(sum("amount")).collect()[0][0]
df.withColumn("percentage", (col("amount") / lit(total)) * 100).show()
```

## Configuration

```python
# Set Spark configuration
spark.conf.set("spark.sql.shuffle.partitions", "200")
spark.conf.set("spark.sql.adaptive.enabled", "true")

# Get configuration
spark.conf.get("spark.sql.shuffle.partitions")

# Set log level
spark.sparkContext.setLogLevel("ERROR")
```
