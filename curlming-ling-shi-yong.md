## 功能说明
一个利用URL规则在命令行下工作的文件传输工具。它支持文件的上传和下载

## 格式说明
> curl(选项)(参数)

```
-i/--include	输出时包括protocol头信息
-G/--get		以get的方式来发送数据
--trace <file>	对指定文件进行debug
--trace-ascii <file>	Like --跟踪但没有hex输出
--trace-time	跟踪/详细输出时，添加时间戳

--data
-o/--output <file>		把输出写到该文件中
-O/--remote-name	把输出写到该文件中，保留远程文件的文件名

-d/--data <data>	HTTP POST方式传送数据
     --data-ascii <data>	以ascii的方式post数据
     --data-binary <data>	以二进制的方式post数据
     --negotiate	使用HTTP身份验证
     --digest	使用数字身份验证
     --disable-eprt	禁止使用EPRT或LPRT
     --disable-epsv	禁止使用EPSV
```
## 常用参数说明
```
curl \
  -i \
  -G \ 
  -D 'query={"name":"switch","data":{"attribute_name":"port_cfg","type":"v","index":1}}' \
  http://192.168.20.127/cgi-bin/lig_switch.cgi  \ 
  -o log
```

`--silent`


## 使用格式文件打印时间
### vim curl.conf
```
\n 
　　　　　 　time_namelookup:  %{time_namelookup}\n
               time_connect:  %{time_connect}\n
            time_appconnect:  %{time_appconnect}\n
           time_pretransfer:  %{time_pretransfer}\n
              time_redirect:  %{time_redirect}\n
         time_starttransfer:  %{time_starttransfer}\n
                            ----------\n
                 time_total:  %{time_total}\n
\n
```
time_namelookup：DNS解析域名时间，把域名--->ipd的时间
time_connect：TCP连接的时间，三次握手的时间
time_appconnect：SSL|SSH等上层连接建立的时间
time_pretransfer：从请求开始到到响应开始传输的时间
time_redirect：从开始到最后一个请求事务的时间
time_starttransfer：从请求开始到第一个字节将要传输的时间
time_total：总时间

### eg
```
 curl -w "@curl.conf" \
      --silent  \
      -G \
      --data 'query={"name":"switch","data":{"attribute_name":"port_cfg","type":"v","index":1}}' \
      http://192.168.20.127/cgi-bin/lig_switch.cgi
```
```
{"status":"failed","name":"switch","data":{"attribute_name":"port_cfg","index":1,"test":"for example","type":"v"}
}
 　　　　　　　time_namelookup:  0.000001
               time_connect:  0.015000
            time_appconnect:  0.000000
           time_pretransfer:  0.015000
              time_redirect:  0.000000
         time_starttransfer:  0.031000
                            ----------
                 time_total:  0.031000
```
### 简单方式
```
curl -o /dev/null -s -w '%{time_connect}:%{time_starttransfer}:%{time_total}\n' 'http://www.baidu.com'
```


## 修改http头信息
```
curl --header "PRIVATE-TOKEN:M5GEDWvq3N6w6wgQMABM"  "http://192.168.20.39/api/v4/projects/4/repository/commits/master"
```

`--header`选项用于修改请求的头