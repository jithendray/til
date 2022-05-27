# Redshift: Generate DDL Create Table Statements

Sometimes I need to create a table that exists in one database or even one cluster but not in another. I need to know DDL for the table to create it. Manually generating DDL create statements for existing tables is sometimes painful and even error prone as I always miss datatypes or variable lengths.

AWS created some amazing SQL for generating this in Redshift. The SQL - [v_generate_tbl_ddl.sql](https://github.com/awslabs/amazon-redshift-utils/blob/master/src/AdminViews/v_generate_tbl_ddl.sql) creates a new view `admin.v_generate_tbl_ddl`. But an `admin` schema is needed prior to running the SQL script.

### Steps: 

1. Create the `admin.v_generate_tbl_ddl` view one time:

    - Create an 'admin' schema, if necessary:
  ```sql
  CREATE SCHEMA admin AUTHORIZATION dwadmin;
  ```

    - Run the `v_generate_tbl_ddl.sql` script to create the DDL-creating view:
  ```sql
  \i v_generate_tbl_ddl.sql
  ```

2. Now a DDL can be generated for all tables in a schema, or for specific tables:
```sql
SELECT DDL
  FROM admin.v_generate_tbl_ddl
 WHERE schemaname = 'SCHEMA_NAME'
   AND tablename IN ('TABLE_NAME_1', 'TABLE_NAME_2')
```

The comments in the header of the `v_generate_tbl_ddl.sql` 
script are helpful for more usage tips and constraints.


## Is there a other way?

Yes, using particular third-party SQL clients automatically generate DDLs for all tables and they can be viewed whenever needed. One such SQL editor is [DBeaver](https://dbeaver.io/). 

> DBeaver is a SQL client software application and a database administration tool. For relational databases it uses the JDBC application programming interface to interact with databases via a JDBC driver. For other databases it uses proprietary database drivers.

![](/img/dbeaver_ddl.png)


------**------