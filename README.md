# DataRestoreTree
数据恢复技术


<pre>
Binlog分类
      binlog分3种格式
         1）Statement: 会在binlog中记录每一条执行修改操作SQL语句的详细信息；
                       优点：
                           不需要记录每一行的变化，减少了binlog日志量，节省IO
         2）Row：  会在binlog中记录每一修改语句的详细信息，包括数据在修改前和修改后的
                   数据的具体信息，好处是会清晰记录每一条修改的详细信息，不好的地方是
                   会产生大量的日志。
         3）Mixed： 这种格式实际上就是Statement和Row的结合体，如果遇到表结构变更就会
                   以Statement来记录，如果涉及语句修改那么就以Row格式记录。
           
         my.cnf配置 binlog_format=row
</pre>

<pre>
数据恢复
      方法1：
           使用开源框架binlog2sql : https://github.com/danfengcao/binlog2sql

           python /binlog2sql/binlog2sql.py --flashback 
           -h127.0.0.1 -P3306 -uroot -p'123456' 
           -dlocal -tuser 
           --start-file='mysql-bin.000038' 
           --sql-type=DELETE 
           --start-datetime='2017-12-17 19:39:33' 
           --stop-datetime='2017-12-17 19:40:01' 
           >/**/data6.sql

       方法2：
           借助mysql自带的binlog解析工具mysqlbinlog，在mysql/bin下

           mysqlbinlog --base64-output=decode-rows -v 
           --start-datetime="2017-12-15 17:48:49" 
           --stop-datetime="2017-12-16 23:59:49" 
           /usr/local/mysql/mysql-bin.000038 >/**/data.sql

           binlog文件默认是base64加密过的，所需需要加上-base64-
</pre>