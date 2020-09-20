1.数据库
	1.mysql 创建 seata 数据库
	2.run /sql/db_seata.sql

2.seata-server
	1.下载并解压 https://github.com/seata/seata/releases	
	2.修改 /conf/file.conf  
		2.1 mode = "db"
		2.2 数据库用户名和密码
	3.启动 sh ./bin/seata-server.sh -p 8091 -h 127.0.0.1 -m db

3.nacos
	1.下载并解压 https://github.com/alibaba/nacos/releases/tag/1.3.2
	2.启动 sh /bin/startup.sh -m standalone
	3.访问 http://127.0.0.1:8848/nacos/index.html   默认用户名和密码: nacos

4.依次启动服务
	1.samples-account
	2.samples-order
	3.samples-storage
	4.samples-business	
	5.检查状态 Nacos: http://127.0.0.1:8848/nacos/#/serviceManagement

5.测试
1.pay request
curl -H "Content-Type:application/json" -X POST -d '{"userId":"1","commodityCode":"C201901140001","name":"风扇","count":2,"amount":"100"}' localhost:8104/business/dubbo/buy	

2.rollback request
enter samples-business , change  BusinessServiceImpl, uncomment the following code ：

```
if (!flag) {
  throw new RuntimeException("测试抛异常后，分布式事务回滚！");
}
```

restart the  samples-business module, and execute the step 5.1


