# ProcessForce objects (User-Defined Objects)

Here you can find SQL queries that return information on all of the objects (tables, fields) of the ProcessForce solution.

---

To display all header fields of Master Data / Document of ProcessForce, use the following SQL query:

```sql
-- Object title (header) fields
 
SELECT U."Code" AS "UDO Code", U."Name" AS "UDO Name", UTB."TableName" AS "UDO Table Name",
F."AliasID" AS "Field", F."Descr" AS "Field Name"
FROM OUDO U
INNER JOIN OUTB UTB on U."TableName"=UTB."TableName"
INNER JOIN CUFD F ON F."TableID" = CONCAT('@',UTB."TableName")
WHERE UTB."TableName" LIKE 'CT_PF_%'
ORDER BY U."Code", F."FieldID";
```

To display all fields of matrix rows of ProcessForce, use the following SQL query:

```sql
-- Object row fields
  
SELECT U."Code" AS "UDO Code", U."Name" AS "UDO Name", UP."TableName" AS "UDO Table Name",
F."AliasID" AS "Field", F."Descr" AS "Field Name"
FROM OUDO U
INNER JOIN UDO1 UP on U."Code"=UP."Code"
INNER JOIN OUTB UTB on U."TableName" = UTB."TableName"
INNER JOIN OUTB UPTB on UP."TableName" = UPTB."TableName"
INNER JOIN CUFD F ON F."TableID" = CONCAT('@',UPTB."TableName")
WHERE UTB."TableName" LIKE 'CT_PF_%'
ORDER
BY U."Code", UPTB."TableName",  F."FieldID"
```
