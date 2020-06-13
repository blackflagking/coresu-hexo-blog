
SQL Server采用linked server建立数据库与数据库之间的连接，功能与Oracle的dblink相似。
连接Oracle需要<span style="color:red">OraOLEDB.oracle</span>接口，但sql server本身并不默认安装，需要手动安装oracle客户端。

![](01.PNG)




文章大致分为：

> * 安装Oracle客户端 
> * 建立linked server



### 1.安装Oracle客户端



![](02.PNG)

##### 下载地址：

[Oracle官网下载地址：https://www.oracle.com/database/technologies/112010-win64soft.html](https://www.oracle.com/database/technologies/112010-win64soft.html)


打开压缩包找到setup.exe右键打开，或以管理员身份运行；


![](03.PNG)

可能会出现不满足要求的情况；
<div style="color:red">注意：可能需要等待很长时间才能弹出此弹窗，不要以为系统无响应或者下错软件了。</div>
</br>

![](04.PNG)


修改解压该路径下/client/stage/cvu下的两个xml文件便可以解决此问题；
```
cvu_prereq.xml
oracle.client_InstantClient.xml
```


![](05.PNG)


```
	 <OPERATING_SYSTEM RELEASE="6.2">
			<VERSION VALUE="3"/>
			<ARCHITECTURE VALUE="64-bit"/>
			<NAME VALUE="WindowsServer2012R2"/>
			<ENV_VAR_LIST>
					<ENV_VAR NAME="PATH" MAX_LENGTH="1023" />
			</ENV_VAR_LIST>
		</OPERATING_SYSTEM>
```

在两个xml文件的图片所示位置追加上述代码部分；

<div style="color:red">
注意：
</br>
1.RELEASE="6.2"而不是RELEASE="6.1"
</br>
2.VALUE="WindowsServer2012R2"写的值应该是系统的版本
</div>
</br>


![](06.PNG)

![](07.PNG)

选择管理员模式；

![](08.PNG)

![](09.PNG)

最好，自己建立一个文件夹，存放oracle客户端软件以便后期管理。
本例为：C:\oracle_client

![](10.PNG)

![](11.PNG)


![](12.PNG)

安装oracle client后，OraOLEDB.Oracle接口会被sql server自动识别。

![](13.PNG)



### 2.建立linked server

```
-- 建立连接服务器
EXEC sp_addlinkedserver
@server='CORESU_LINK',	--被访问的服务器别名
@srvproduct='',	--SqlServer默认不需要写，或ORACL
@provider='OraOLEDB.Oracle',	--数据库访问接口
@datasrc='192.168.1.200:1521/prod'	--要访问的数据库相关信息
GO

-- 映射登陆用户
EXEC sp_addlinkedsrvlogin
@rmtsrvname='CORESU_LINK',	--被访问的服务器别名
@useself='false',	--固定这么写
@rmtuser='scott',	--被访问的数据库用户名
@rmtpassword='scott'	--被访问的数据库用户密码
GO

-- 测试查询
select * from CORESU_LINK..SCOTT.EMP;

-- 删除连接服务器
EXEC sp_dropserver "CORESU_LINK"
```
<div style="color:red">
注意：连接服务器名、oracle用户名、表名均要大写。
</div>


![](14.PNG)