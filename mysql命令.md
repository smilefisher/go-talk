#### 1 mysql新建用户，分配权限

```sql
-- 1 新建用户
 create user 'chen'@'%' identified by '123456';
 
 CREATE USER 'chen'@'10.246.34.85' IDENDIFIED BY '123456';
 
-- 2 赋予所有网站访问权限
 GRANT ALL ON *.* TO 'chen'@'%';
 
 GRANT SELECT, INSERT ON test.user TO 'chen'@'%';

```



#### 2 mysql sysbench实例

1. 官网 https://github.com/akopytov/sysbench

2. 安装

   ```shell
   # centos
   curl -s https://packagecloud.io/install/repositories/akopytov/sysbench/script.rpm.sh | sudo bash
   sudo yum -y install sysbench
   ```



3. sysbench的OLTP基准测试

   ```shell
   #1 准备压测数据
   sysbench /usr/share/sysbench/oltp_insert.lua  --mysql-host=127.0.0.1  --mysql-port=3306 --mysql-user=chen  --mysql-password='123456' --mysql-db=test-oltp --db-driver=mysql  --tables=15  --table-size=500000  --report-interval=10 --threads=128   --time=120 prepare
   
   
   # 2 压测
   sysbench /usr/share/sysbench/oltp_insert.lua   --mysql-host=127.0.0.1  --mysql-port=3306 --mysql-user=chen  --mysql-password='123456'  --mysql-db=test-oltp --db-driver=mysql  --tables=15  --table-size=500000  --report-interval=10 --threads=128   --time=120 run
   也可保存结果到文件
    <command> >> ./mysysbench.log
    
   # 3 清理压测数据
   sysbench /usr/share/sysbench/oltp_insert.lua   --mysql-host=127.0.0.1  --mysql-port=3306 --mysql-user=chen  --mysql-password='123456'  --mysql-db=test-oltp --db-driver=mysql  --tables=15  --table-size=500000  --report-interval=10 --threads=128   --time=120 cleanup
   
   ```

   

![image-20201116223418511](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201116223418511.png)