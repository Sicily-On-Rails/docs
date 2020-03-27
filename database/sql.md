# What is SQL? 
SQL (pronunced as the letter S-Q-L or as sequel) is an abbrevevation for Structured Query Language.

### The SELECT Statement

```sql
SELECT * 
FROM companies;
```
---
### Understanding Data Insertion
#### Inserting complete rows
The simplest way to insert data into a table is to use the basic ***INSERT*** sintax, which requires that you specify the table name and values to be inserted into the new row.
``` sql
INSERT INTO companies
VALUES(1,
       'AMV Idealab',
        current_timestamp,
        current_timestamp
        )
```
---
### Updating and Deleting Data
*In thi lesson, you will learn how to use the* ***UPDATE*** *and* ***DELETEE*** *statements to enable you to further manipulate your table data*

#### Updating data
``` sql
UPDATE companies
SET name = 'AMV Idealab'
WHERE company_id = 1;
```

#### Deleting Data
``` sql
DELETE FROM companies
WHERE company_id = 2;
```
---
### Creating and Manipulating Table
