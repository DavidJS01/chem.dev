+++
title = "Snowflake Schemas vs Star Schemas: a TLDR"
date = 2024-11-04
draft = false
[extra]
summary = "Comparing data warehouse schema designs"

[taxonomies]

tags = ["data modeling"]
+++
> TLDR: Star schemas have denormalized dimension tables, snowflake schemas have normalized dimension tables. Probably favor star schemas unless you really need a snowflake schema.

# TLDR Version
Use star schemas if:
- Need fast query performance
- Storage is not a concern
- Want simpler maintenance

Use snowflake schemas if:
- Data consistency in your warehouse is critical
- Need to optimize for storage

| Concern   | Star Schemas (Denormalized)          | Snowflake Schemas (Normalized)              |
|:-----------------:|:------------------------------------:|:-------------------------------------------:|
| Storage           | More redundant, higher storage needs | Efficient storage                           |
| Integrity         | Potential inconsistencies            | Overall better integrity from normalization |
| Query Performance | Faster (fewer joins)                 | Slower (more joins needed)                  |

# Longer Version
##  Star Schemas
Star schemas are the most common data model used in [OLAP](https://en.wikipedia.org/wiki/Online_analytical_processing) systems. This includes examples like Snowflake, Redshift, Clickhouse, etc.
  
A star schema is composed of two different types of tables: **fact tables** and **dimension tables**.

#### Fact Tables
In a fact table, each row represents an event that happened at a particular time.
   - Some columns in the table are attributes
	 - Cost of buying an item, the price of the item sold, etc
   - Other columns are foreign keys to **dimension tables** 
#### Dimension Tables
Dimension tables are used to represent the ***who, what, where, when, how, and why*** of the event. If we had a "sales" fact table, it might have these dimensions:
     
| Dimension Type | Example Dimension | Example Dimension Columns     |
|:--------------:|:-----------------:|-------------------------------|
| who            | customer          | customer\_name, location, phone         |
| what           | product           | product\_name, category, brand |
| where          | store             | location, region, market      |
| when           | time              | date, month, year, season     |
| how            | shipping          | delivery\_method, carrier      |
| why            | promotion         | promotion\_name, discount\_type |

## Snowflake Schemas
> The only difference between a snowflake schema and a star schema is that the snowflake schema's dimension tables are [normalized](https://www.youtube.com/watch?v=GFQaEYEc8_8).

Consider a simple sales database using a snowflake schema with a sales fact table, product dimension, and a category dimension (pictured below).

By normalizing the product dimension and referencing categories by id, any updates for a product's category (moving "Electronics" to "Consumer Electronics") only needs to happen in one place: the category dimension.

![diagram of a snowflake schema](/snowflake_schema_example.png)

This results in:

**Stronger data integrity**
   - Updates to category information happen in one place
   - Can't accidentally have "Electronics" and "electronics" as different categories

**Better storage efficiency**
   - Category details stored once, not repeated for every product

**Single source of truth for data**
   - Each piece of information exists in exactly one place
   - Easier to enforce business rules



