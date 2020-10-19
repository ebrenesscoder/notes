# 2020-10-19

## ERROR
```
Access denied for user 'root'@'%' to database 'crafter'
Query is : GRANT ALL PRIVILEGES ON crafter.* TO 'crafter'@'localhost'
WITH GRANT OPTION
```

## CAUSE
You can't grant `SUPER` privilege on RDS, so you CANNOT use the `ALL PRIVILEGES` shortcut.
You must name the privileges you want to grant explicitly, even if it means listing every privilege except SUPER.
You can find out what privileges you have with
```sql
 SHOW GRANTS FOR root
```
