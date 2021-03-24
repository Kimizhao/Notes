### SQL Server-参考引用

#### 1.[SQL使用SELECT * [除了columnA] FROM tableA排除列？](https://www.itranslater.com/qa/details/2104903559677477888)

https://stackoverflow.com/questions/729197/sql-exclude-a-column-using-select-except-columna-from-tablea

```
/* Get the data into a temp table */
SELECT * INTO #TempTable
FROM YourTable
/* Drop the columns that are not needed */
ALTER TABLE #TempTable
DROP COLUMN ColumnToDrop
/* Get results and drop temp table */
SELECT * FROM #TempTable
DROP TABLE #TempTable
```
