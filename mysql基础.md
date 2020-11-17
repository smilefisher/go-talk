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

   1. 执行结果如下图

![image-20201116223418511](E:\github\go-offer\images\image-20201116223418511.png)





#### 3 mysql 表设计选择优化的数据类型

1. 更小的通常更好，选择满足需求的最小数据类型
2. 简单就好，比如int比string更好，内建数据类型比string好(不要用string替代date)，使用int替代ip
3. 避免使用null, 更难优化，null被索引是需要额外的字节



#### 4 mysql整形一个特殊的问题？int(11)，11对大部分应用无意义，他不会限制合法的取值范围，只是规定交互工具使用，例如命令行客户端.



#### 5 mysql 数据类型

1. **INT** tinyint(8bit), smallint(16), mediumint(24), int(32), bigint(64), 设计表是指定（int(11)）, 长度11只是指定客户端工具显示的宽度，实际并不影响存储空间。

2. **浮点数**， float double decimal， 对于金融数据，可以使用bigint，修改存储单位例如分，厘
3. **CHAR, VARCHAR,** varchar需要额外一个或者两个字节记录长度（高性能msyql p115）
4. **BLOB, TEXT**
5. **日期** datetime(8字节), timestamp（4字节）
6. **位数据** bit, set
7. **数字比字符串快**，ip 转 int (select INET_ATON('192.178.1.1')) , select INET_NTOA('3232891137')