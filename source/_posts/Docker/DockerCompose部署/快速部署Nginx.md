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
  nginx:
    restart: always
    image: nginx
    container_name: nginx
    ports:
      - 80:80
    volumes:
      - ./conf/nginx.conf:/etc/nginx/nginx.conf
      - ./www/root/public:/usr/share/nginx/www/root
      - ./log:/var/log/nginx/

                                                      
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




