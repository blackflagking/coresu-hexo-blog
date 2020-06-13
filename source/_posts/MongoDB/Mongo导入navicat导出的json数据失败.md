
工作中面对用户复杂的环境，往往会出现，从**向日葵**远程到**跳板机**再到**数据库服务器**的尴尬现象，甚至还会嵌套多层，这就给数据迁移带来了不便。下面价绍最近一次，mongo的数据迁移遇到开发人员贪图<span style='color:red'>省事</span>使用navicat工具导出json格式数据，使用mongoimport工具导入时引发问题的全过程。主要内容如下：
 
-------

> * 还原导出过程
> * 定位问题
> * 问题产生原因
> * 解决问题

![mongo01](01.png)


------

### 1.还原导出过程

下面简要介绍了使用navicat工具导出mongo数据的全过程，

首先，进入导出向导；

![mongo01](01.png)

其次，选择导出文件格式；

![mongo02](02.png)

选择导出文件格式；

![mongo03](03.png)

选择文件导出字段；

![mongo04](04.png)
![mongo05](05.png)
![mongo06](06.png)

开始导出；

![mongo07](07.png)

看到successfully日志，说明导出成功。

![mongo08](08.png)




### 2.定位问题

```
mongoimport -u XXXX -p XXXX -d coresu -c restaurants --type=json 
--file=/home/mongodump/exp_restaurants.json --drop
```
生产库执行上述命令后，日志显示文档已经成功导入，但是在应用端检索不到数据，十分不解。
抱着试一试的心态，执行了db.restaurant.find().count()显示为结果为1，
<span style='color:red'>这很奇怪，导入数据按理来说不应该只有一条数据啊！！！</span>
在经过与数据库开发人员的求证后，该集合确实不只是一行数据，判定问题在数据集。

### 3.问题产生原因
打开导出的json文件可以看到，如果使用navicat工具导出数据会自动给数据最外层加上
```
{
  "RECOREDS" : [
    ...
    ...
    ...
  ]
}
```
而且，每条json格式的记录之间还会自动添加逗号。

![mongo09](09.png)



正常使用mongoexport命令导出的json文件格式如下：

![mongo10](10.png)

<span style='color:red'>
<p>注意：</p>
1.mongoimport导入json数据格式文件，要求不能有最外层不能有多余包含；
<br>
2.每条json数据之间，不以逗号分隔。
</span>

### 4.解决问题
 
```
mongoexport -u XXXX -p XXXX -d coresu -c restaurants --type=json 
-o /home/mongodump/exp_restaurants.json
```
最后使用mongo官方工具mongoexport导出，传到生产库成功导入。

