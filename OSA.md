# OSA

## Segment

~~~sql
SELECT * FROM NFS.dbo.filing_index WHERE filing_status_id =50
~~~

download程序会将edgar html 的segment filing  [filing_status_id] 标记为50

```sql
SELECT top 100 * FROM  MEDS.dbo.dcms_auto_locate_info and SystemCategory =20
```

这是记录automation整个流程的主表，filing_status_id =50 的filing 会被automation 程序分发到这张表，其中

status :

- 0  初始化状态
- 5  完成
- 7  失败



```sql
SELECT top 100 r.DocumentId FROM segment.segment_value v LEFT JOIN segment.segment_report r ON r.ReportId = v.ReportId
WHERE r.DocumentId IN 
(?) AND r.CreatorId =57
```

检验segment  autoparse之后是否有值mapping上，CreatorId =57表示是程序执行的

~~~sql
  SELECT * FROM NFS.dbo.filing_index WHERE collection_category_id = 20 AND document_id = ? and  company_id= ? " ;
~~~

  检验Segment filing 是否会激活 filing_status_id =1

​    

## NAV

```sql
 SELECT * FROM MEDS.dbo.log_ext_index WHERE SourceFileId IN (?)
```

这个document download 下来的主表 

```sql
SELECT  top 10 * FROM MEDS.dbo.log_parse_distribute  WHERE FileId IN (?)
```

这里的fileId 是meds 的，把meds 的filing 分发的表，分发到不同的服务器做meds的autoprse

meds autoparse完之后，会根据opm是否需要来生成nav 这个数据点，这时数据库中会存在

```sql
SELECT * FROM MEDS.dbo.log_ext_index WHERE SourceFileId IN (?) AND SystemCategory =25
```

这条记录

接下来选取us市场的走classification 流程

```sql
SELECT * FROM MEDS..dbo.dcms_log WHERE IsTrue =1 AND FileId IN (?)
```

如果该条记录存在，表示有keyword 会插入以下记录：

```sql
SELECT TOP 100 * FROM MEDS.dbo.filing_remark_log WHERE CreatorId =57
```

这是NAV classification 之后的结果，这些case都会进入automation,是html 文件级别的，每个文件都会插入下面这张表

```sql
SELECT top 100 * FROM  MEDS.dbo.dcms_auto_locate_info and SystemCategory =25
```

status :

- 0 初始状态

- 5 autoparse 流程走完
  - 7 失败

  ```sql
     SELECT * FROM aor_ms_data_point WHERE FileId IN (
          SELECT FileId FROM log_ext_index WHERE SourceFileId IN (?) 
              AND SystemCategory=25
      )   
  ```

  检验opm autoparse之后是否有值mapping上

  ```sql
  SELECT top 100 *  FROM MEDS..task_list t LEFT JOIN MEDS..log_ext_index l on  l.FileId = t.FileId WHERE l.SourceFileId IN (?)
  AND l.SystemCategory =25
  ```

  检验NAV filing 是否激活 TaskStatus
  
  |      |      |      |
  | ---- | ---- | ---- |
  |      |      |      |
  |      |      |      |
  |      |      |      |
  
  











