## lua文件 

```
#!/usr/bin/lua

function getparam_normal(io,port,value)
	return io..','..port..','..value
end

function getparam_X(io,port,value,str1,str2)
	return (io==0 and "IN" or "OUT")..'.'..str1..'.'..port..'.'..str2..','..value
end


transformation_list=
{
	[1]={k3name="~01@DETAIL-TIMING",param=function(io,port,value) return '6,'..io..','..port..','..value end },
	[2]={k3name="~01@DETAIL-TIMING",param=function(io,port,value) return '7,'..io..','..port..','..value end },
	[3]={k3name="~01@DETAIL-TIMING",param=function(io,port,value) return '8,'..io..','..port..','..value end },
	[4]={k3name="~01@DETAIL-TIMING",param=function(io,port,value) return '9,'..io..','..port..','..value end },
	[5]={k3name="~01@DETAIL-TIMING",param=function(io,port,value) return '10,'..io..','..port..','..value end },
	[6]={k3name="~01@EXT-PCMODE",param=getparam_normal},
	[7]={k3name="~01@EXT-COMPENSATION",param=getparam_normal },
	[8]={k3name="~01@HDCP-MOD",	param=getparam_normal},
	[9]={
                k3name="~01@SIG-TYPE",
                param=function(io,port,value) 
                        if value==1 then 
                                value=2 
                        elseif value==2 then 
                                value=1
                        else
                                value=7 --0.auto
                        end

                        return io..','..port..','..value end 
        },
	[10]={k3name="~01@EXT-VID-CS",param=getparam_normal},
	[11]={k3name="~01@EXT-VID-DC",param=getparam_normal},
	[12]={k3name="~01@VID-PATTERN",param=getparam_normal},

	[97]={k3name="~01@AUD-DELAY",param=getparam_normal},
	[98]={k3name="~01@X-AUD-EMB",param=function(io,port,value) return (io==0 and "IN" or "OUT")..'.HDMI.'..port..'.AUDIO.1,'..value end },
}


--[[
	151~...use key [subfunc_id:boardtype:io]
--]]

	--[18]=VGAA

	transformation_list['152:18:0']={k3name="~01@X-VGA-OFFSET",param=function(io,port,value) return (io==0 and "IN" or "OUT")..'.HDMI.'..port..'.VIDEO.1,'..value end };
	transformation_list['152:18:1']={k3name="~01@X-VGA-SYNC",  param=function(io,port,value) return (io==0 and "IN" or "OUT")..'.HDMI.'..port..'.VIDEO.1,'..value end };


	transformation_list['158:41:*']={k3name="~01@X-AUD-EMB",param=function(io,port,value) return (io==0 and "IN" or "OUT")..'.ANALOG_AUDIO.'..port..'.AUDIO.1,'..value end };
	transformation_list['158:42:*']=transformation_list['158:41:*'];
	transformation_list['158:46:*']=transformation_list['158:41:*'];

------------------------------------------------------------------------------

function transformation_liga_to_k3000(subfunc,boardtype,io,port,value)
        local key =subfunc
        if key>150
        then
                key=subfunc..':'..boardtype..':'..io
                if transformation_list[key]==nil
                then
                        key=subfunc..':'..boardtype..':*'
                end
        end
        if transformation_list[key]==nil
        then
                return nil
        end
        return transformation_list[key].k3name.." "..transformation_list[key].param(io,port,value)
end


--[[
-- test
for key=1 ,256,1 
do
        for btype=0,(key>150 and 256 or 0),1
        do
                local res;

	        res=transformation_liga_to_k3000(key,btype,0,1,0)
                if res~=nil
                then 
                        print('index=['..key..':'..btype..']',res)
                end

	        res=transformation_liga_to_k3000(key,btype,1,1,0)
                if res~=nil
                then 
                        print('index=['..key..':'..btype..']',res)
                end

	        res=transformation_liga_to_k3000(key,btype,0,1,1)
                if res~=nil
                then 
                        print('index=['..key..':'..btype..']',res)
                end

	        res=transformation_liga_to_k3000(key,btype,1,1,125)
                if res~=nil
                then 
                        print('index=['..key..':'..btype..']',res)
                end
        end
end
--]]

```

# C接口
```

int lig_prtl_liga80_2_k3_init(void)
{
	m_lua_handle = luaL_newstate();
	luaopen_base(m_lua_handle); //
	luaopen_table(m_lua_handle); //
	luaopen_package(m_lua_handle); //
	luaopen_io(m_lua_handle); //
	luaopen_string(m_lua_handle); //
	luaL_openlibs(m_lua_handle); //打开以上所有的lib
	
	if (luaL_dofile(m_lua_handle,LUA_CONFIG_FILE))
	{
		printf("Failed to run lua code %s\n",LUA_CONFIG_FILE);
		return -1;
	}
	
	return 0;
	
}
int lig_prtl_liga80_2_k3_destroy(void)
{
	lua_close(m_lua_handle);
	return 0;
}

int lig_prtl_liga80_2_k3(OBJ_LIG_A_FUNCID * funcinfo,char * buff,size_t bufflen)
{

	//funcinfo->func_id+funcinfo->type=buff;


	//luaL_dostring 等同于luaL_loadstring() || lua_pcall()
	//注意：在能够调用Lua函数之前必须执行Lua脚本，否则在后面实际调用Lua函数时会报错，
	//错误信息为:"attempt to call a nil value."


	//function transformation_liga_to_k3000(subfunc,boardtype,io,port,value)
	lua_getglobal(m_lua_handle,LUA_GLOBAL_GET_K3);
	
	lua_pushinteger(m_lua_handle,funcinfo->func_id);
	lua_pushinteger(m_lua_handle,funcinfo->type);
	lua_pushinteger(m_lua_handle,funcinfo->io);
	lua_pushinteger(m_lua_handle,funcinfo->port);
	lua_pushinteger(m_lua_handle,funcinfo->func_value);
	
	//下面的第二个参数表示带调用的lua函数存在两个参数。
	//第三个参数表示即使带调用的函数存在多个返回值，那么也只有一个在执行后会被压入栈中。
	//lua_pcall调用后，虚拟栈中的函数参数和函数名均被弹出。
	if (lua_pcall(m_lua_handle,5,1,0))
	{
		printf("error is %s.\n",lua_tostring(m_lua_handle,-1));
		return -1;
	}
	//此时结果已经被压入栈中。
	if (!lua_isstring(m_lua_handle,-1))
	{
		printf("function must return a string.\n");
		return -1;
	}
	char * ret = lua_tostring(m_lua_handle,-1);
	
	lua_pop(m_lua_handle,-1); //弹出返回值。
	
	if(bufflen>=strlen(ret)+1)
		strncpy(buff,ret,strlen(ret)+1);
	else
		return -2;

	return strlen(buff);
}
```