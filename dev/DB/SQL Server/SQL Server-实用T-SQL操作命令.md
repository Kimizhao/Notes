### SQL Server-实用T-SQL操作命令

#### 查拥堵

```
SELECT request_session_id as sessionID,
       resource_type,
       DB_NAME(resource_database_id) AS DatabaseName,
       OBJECT_NAME(resource_associated_entity_id) AS TableName,
       request_mode,
       request_type,
       request_status,
       SUBSTRING (qt.text, er.statement_start_offset/2,
                                        (CASE WHEN er.statement_end_offset = -1 THEN LEN(CONVERT(NVARCHAR(MAX), qt.text)) * 2
                                                    ELSE er.statement_end_offset END - er.statement_start_offset)/2) as CurrentSQL,
       qt.text,
       er.start_time,
             DATEDIFF ( millisecond,er.start_time, GETDATE() ) AS  execTime
FROM sys.dm_tran_locks AS TL
   JOIN sys.all_objects AS AO ON TL.resource_associated_entity_id = AO.object_id
   inner join sys.dm_exec_requests er on tl.request_session_id = er.session_id
   cross apply sys.dm_exec_sql_text(er.sql_handle)as qt
WHERE request_type = 'LOCK'
   AND request_status = 'GRANT'
   --AND request_mode IN ('X','S')
   AND AO.type = 'U'
   AND resource_type = 'OBJECT'
   AND TL.resource_database_id = DB_ID();
```
