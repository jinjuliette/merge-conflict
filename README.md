# merge-conflict

Resolving a merge conflict between two companies' data models, in Databricks.

# Overview

## The context

A sports equipment manufacturer with a mature data infrastructure (OLTP + OLAP across bronze/silver/gold) acquires an energy bar startup whose data is scattered and dirty. Management rules out a multi-year migration: what's needed is a unified data layer in the short term. We have 3 objectives:

- an aggregated, reliable dashboard
- a technology the team can pick up quickly
- and a solution that scales

Throughout, the acquiring company is referred to as the **parent** and the acquired company as the **child**.

**Stack:** Databricks Free Edition, Python/PySpark, SQL, AWS S3, medallion architecture, Databricks dashboards.

## The steps

**1. Understand the data model**

The parent has a clean star schema: `dim_customers`, `dim_products`, `gross_price`, `fact_orders`. The child has the same entities but with different column names, no `division` column, the variant baked into the product name, and a range of errors (duplicates, negative values, "unknown", stray whitespace, typos). Each company ships two folders: _full load_ (history through 30 November) and _incremental load_ (December).

**2. Create the Databricks Free Edition account**

**3. Set up the catalog**

`CREATE CATALOG fmcg`, then the `bronze`, `silver` and `gold` schemas. The parent's four CSVs are imported manually straight into `gold`, simulating a pipeline that would already exist in production. Customer and product codes are cast to string, since they are categories rather than numbers.

**4. Generate the `dim_date` table** with a Python/Spark script.

**5. Create an AWS account and an S3 bucket**, then upload the child's files to S3.

**6. Connect Databricks to S3**

**7. Process the child dimensions**

_Bronze:_ read the CSVs from S3, add metadata columns (`read_timestamp`, `file_name`, `file_size`) for traceability, write as Delta with Change Data Feed enabled.

---

_Based on the Databricks end-to-end data engineering project tutorial by [codebasics](https://www.youtube.com/@codebasics) (Dhaval Patel)._
