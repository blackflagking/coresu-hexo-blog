---
title: 【RAC】RAC安装过程提示not a shared subnet错误 
date: 2019-10-27 
categories: Oracle
tags: Oracle-RAC
---



### 问题阐述：  


在安装RAC句群过程中，出现INS-41107错误导致安装无法进行。   
具体错误提示已经在截图中用红色横线标出。
![错误截屏](./RAC安装子网错误/子网掩码问题.png )

### 解决办法：  
经过各种排查，发现在“节点1”的ifcfg-eth0文件中的NETMASK拼写错误导致的在RAC安装过程中无法继续进行。


![](./RAC安装子网错误/ifcfg-eth0子网掩码.png )

由于"节点2"的ifcfg-eth1的文件是由"节点1"scp过来的，故也需要更改。  


![](./RAC安装子网错误/ifcfg-eth1子网掩码.png )

更改完毕后RAC集群便可正常进行安装。   
注意：修改完毕后需要重启网络服务service network restart，重新启动图形化界面安装RAC，才能生效。


