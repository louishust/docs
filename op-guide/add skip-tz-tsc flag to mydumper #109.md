We have exported data from SQL by using `mydumper`. Details as below.

```bash
+./bin/mydumper -h 127.0.0.1 -P 3306 -u root -t 16 -F 128 -B test -T t1,t2 --skip-tz-utc -o ./var/test
```

We use `-B test` to indicate that these operations go to the database `test` and use `-T t1,t2` to show that only two tables, `-T t1,t2`, are exported.

`-t 16` means we use 16 threads to export data. `-F 128` refers to cutting an actual table into chunks, each of which is 128MB in size.

+ The purpose of adding the parameter `--skip-tz-utc` is to ignore the inconsistency of time zone setting between MySQL and the data exporting machine as well as to disable automatic conversion.
+
 > Note: In some clouds that need `super privilege`, such as Aliyun, `mydumper` needs to be added with the parameter `--no-locks`. Otherwise, you don't have the priviledge to perform this action.
 
### Export data to TiDB
