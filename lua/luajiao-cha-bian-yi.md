# 获取源码
```
curl -R -O http://www.lua.org/ftp/lua-5.3.5.tar.gz
tar zxf lua-5.3.5.tar.gz
cd lua-5.3.5
```

# 修改Makefile

### 修改安装路径
vi Makefile
```
INSTALL_TOP= /usr/local
```
改为
```
INSTALL_TOP= 自定义安装路径
```

### 修改编译配置
**注意只修改它表明可以修改的位置**
vi src/Makefile
```
# == CHANGE THE SETTINGS BELOW TO SUIT YOUR ENVIRONMENT =======================

# Your platform. See PLATS for possible values.
PLAT= none

CC= gcc -std=gnu99
CFLAGS= -O2 -Wall -Wextra -DLUA_COMPAT_5_2 $(SYSCFLAGS) $(MYCFLAGS)
LDFLAGS= $(SYSLDFLAGS) $(MYLDFLAGS)
LIBS= -lm $(SYSLIBS) $(MYLIBS) 

AR= ar rcu
RANLIB= ranlib
RM= rm -f

SYSCFLAGS=
SYSLDFLAGS=
SYSLIBS=

MYCFLAGS=
MYLDFLAGS=
MYLIBS=
MYOBJS=

# == END OF USER SETTINGS -- NO NEED TO CHANGE ANYTHING BELOW THIS LINE =======
```
修改后
```
# == CHANGE THE SETTINGS BELOW TO SUIT YOUR ENVIRONMENT =======================

# Your platform. See PLATS for possible values.
PLAT= none

CC= powerpc-linux-gcc -std=gnu99
CFLAGS= -O2 -Wall -Wextra -DLUA_COMPAT_5_2 $(SYSCFLAGS) $(MYCFLAGS)
LDFLAGS= $(SYSLDFLAGS) $(MYLDFLAGS)
LIBS= -lm $(SYSLIBS) $(MYLIBS) 

AR= powerpc-linux-ar rcu
RANLIB= powerpc-linux-ranlib
RM= rm -f

SYSCFLAGS=
SYSLDFLAGS=
SYSLIBS=

MYCFLAGS=
MYLDFLAGS=
MYLIBS=
MYOBJS=

# == END OF USER SETTINGS -- NO NEED TO CHANGE ANYTHING BELOW THIS LINE =======
```

# 安装

修改完成后执行
```
make linux 
make install
```
安装后的安装目录
```
├── bin
│   ├── lua
│   └── luac
├── include
│   ├── lauxlib.h
│   ├── luaconf.h
│   ├── lua.h
│   ├── lua.hpp
│   └── lualib.h
├── lib
│   ├── liblua.a
│   └── lua
│       └── 5.3
├── man
│   └── man1
│       ├── lua.1
│       └── luac.1
└── share
    └── lua
        └── 5.3
```

拷贝到开发板上即可

#　测试:
开发板上运行
```
chmod +x lua
./lua -v
echo "print(\"hello lua\")" | ./lua 
```

# 注意

**`make test`**不能在PC上执行,因为是交叉编译的所以无法执行测试

**可能遇到缺少库的问题,不过通常都能在交叉编译器的路径中找到,拷贝到板子上就行**
