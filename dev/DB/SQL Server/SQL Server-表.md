### SQL Server-表

#### 修改列

```mssql
ALTER TABLE dbo.doc_exy ALTER COLUMN column_a DECIMAL (5, 2) ;  
GO  
```



#### sql中同一个表一个字段的值赋值给另一个字段

```sql
UPDATE SG_User  SET DefaultOrganizationID = OrganizationID
GO
```

