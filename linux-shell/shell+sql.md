## sqlite3 选项
- -header 显示表头
- -column 列显示

## echo + sqlite3

实现单句sql的快速查询,结果返回到标准输出,错误返回到标准错误,可以通过重定向再编辑
```
echo "select * from ?? ;" | sqlite3 -header -column  [DB file]
```

## cat+ sqlite3
可以一次执行多条语句
```
cat<<EOF | sqlite3 -header -column  [DB file]
    sql
    sql
    sql
EOF
```
