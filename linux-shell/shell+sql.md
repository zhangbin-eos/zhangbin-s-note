## echo + sqlite3
实现单句sql的快速查询,结果返回到标准输出,错误返回到标准错误,可以通过重定向再编辑
```shell
echo "select * from ?? ;" | sqlite3 -header -column  [DB file]
```

cat+ sqlite3
```shell



```