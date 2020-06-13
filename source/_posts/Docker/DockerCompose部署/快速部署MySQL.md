### **文章概览**
> - [是什么？](#是什么？) 
> - [为什么？](#为什么？)  
> - [怎么做？](#怎么做？)  
> - [参考文章](#参考文章)

### **是什么?**

   
### **为什么？**  


### **怎么做？**
1. 配置docker-compose
(1) 选择数据库安装位置
```
mkdir -p /usr/local/docker/mysql/data                

cd /usr/local/docker/mysql                    
```
(2) 编写docker-compose.yml配置文件
```
vi docker-compose.yml                                                    

```
(3) 复制配置信息如下：
```
version: '3'
services:
  db:
    container_name: MySQL
    image: mysql
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: YOUR_MYSQL_ROOT_PASSWORD
    command:
      --default-authentication-plugin=mysql_native_password
      --character-set-server=utf8mb4
      --collation-server=utf8mb4_general_ci
      --explicit_defaults_for_timestamp=true
      --lower_case_table_names=1
      --max_allowed_packet=128M;
    ports:
      - 3306:3306
    volumes:
      - /usr/local/docker/mysql/data:/var/lib/mysql
                                                      
```
(4) 点击Esc键并保存退出；
```
:wq                                                                 
```

2. 启动容器
```
docker-compose up -d
```

3. 滚动启动
```
docker-compose pull && docker-compose up -d
```


### 参考文章   




