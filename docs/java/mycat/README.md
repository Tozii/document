# Mycat

[源码开放地址](https://github.com/MyCATApache/Mycat-Server)

[Mycat2源码开放地址](https://github.com/MyCATApache/Mycat2)

[官方Mycat权威指南手册](http://www.mycat.org.cn/document/mycat-definitive-guide.pdf)

## 介绍
---
Mycat相当于后端数据库的一个代理，客户端连接的是Mycat代理，所有从客户端过来的sql语句都会经过Mycat，Mycat通过配置文件按照指定配置对请求sql做转发等动作。

> Mycat2:客户端发送SQL到mycat2,mycat2拦截对应的SQL执行不同的命令,对于不需要拦截处理的SQL,透传到有逻辑表的mysql,这样,mycat2对外就伪装成mysql数据库

- 一个彻底开源的，面向企业应用开发的大数据库集群
- 支持事务、ACID、可以替代MySQL的加强版数据库
- 一个可以视为MySQL集群的企业级数据库，用来替代昂贵的Oracle集群
- 一个融合内存缓存技术、NoSQL技术、HDFS大数据的新型SQL Server
- 结合传统数据库和新型分布式数据仓库的新一代企业级数据库产品
- 一个新颖的数据库中间件产品

### 关键特性
- 支持SQL92标准
- 支持MySQL、Oracle、DB2、SQL Server、PostgreSQL等DB的常见SQL语法
- 遵守Mysql原生协议，跨语言，跨平台，跨数据库的通用中间件代理。
- 基于心跳的自动故障切换，支持读写分离，支持MySQL主从，以及galera cluster集群。
- 支持Galera for MySQL集群，Percona Cluster或者MariaDB cluster
- 基于Nio实现，有效管理线程，解决高并发问题。
- 支持数据的多片自动路由与聚合，支持sum,count,max等常用的聚合函数,支持跨库分页。
- 支持单库内部任意join，支持跨库2表join，甚至基于caltlet的多表join。
- 支持通过全局表，ER关系的分片策略，实现了高效的多表join查询。
- 支持多租户方案。
- 支持分布式事务（弱xa）。
- 支持XA分布式事务（1.6.5）。
- 支持全局序列号，解决分布式下的主键生成问题。
- 分片规则丰富，插件化开发，易于扩展。
- 强大的web，命令行监控。
- 支持前端作为MySQL通用代理，后端JDBC方式支持Oracle、DB2、SQL Server 、 mongodb 、巨杉。
- 支持密码加密
- 支持服务降级
- 支持IP白名单
- 支持SQL黑名单、sql注入攻击拦截
- 支持prepare预编译指令（1.6）
- 支持非堆内存(Direct Memory)聚合计算（1.6）
- 支持PostgreSQL的native协议（1.6）
- 支持mysql和oracle存储过程，out参数、多结果集返回（1.6）
- 支持zookeeper协调主从切换、zk序列、配置zk化（1.6）
- 支持库内分表（1.6）
- 集群基于ZooKeeper管理，在线升级，扩容，智能优化，大数据处理（2.0开发版）。

## 配置文件

### Mycat2官方安装包默认配置文件(mycat.yml)
```yml
metadata:
  schemas: [{
              schemaName: 'db1' ,   targetName: 'repli',
            }]
interceptors:
  [{
     user: {ip: '.', password: '123456', username: root}
   }]
datasource:
  datasources: [{name: defaultDs, ip: 0.0.0.0,port: 3306,user: root,password: 123456,maxCon: 10000,minCon: 0,
                 maxRetryCount: 3, #连接重试次数
                 maxConnectTimeout: 10000, #连接超时时间 毫秒
                 dbType: mysql, #
                 url: 'jdbc:mysql://127.0.0.1:3306?useUnicode=true&serverTimezone=UTC',
                 weight: 1, #负载均衡权重
                 initSqls: []
                  , #建立连接后执行的sql,在此可以写上use xxx初始化默认database
                 instanceType:,#READ,WRITE,READ_WRITE ,集群信息中是主节点,则默认为读写,副本则为读,此属性可以强制指定可写,
                 initSqlsGetConnection: true
                },
                {name: defaultDs2, ip: 0.0.0.0,port: 3307,user: root,password: 123456,maxCon: 10000,minCon: 0,maxRetryCount: 3,maxConnectTimeout: 10000,dbType: mysql,
                 url: 'jdbc:mysql://127.0.0.1:3307?useUnicode=true&serverTimezone=UTC',weight: 1,initSqls: []
                  ,instanceType:,#READ,WRITE,READ_WRITE
                  initSqlsGetConnection: true
                }
  ]
  datasourceProviderClass: io.mycat.datasource.jdbc.datasourceprovider.AtomikosDatasourceProvider
  timer: {initialDelay: 10, period: 5, timeUnit: SECONDS}
cluster: #集群,数据源选择器,既可以mycat自行检查数据源可用也可以通过mycat提供的外部接口设置设置数据源可用信息影响如何使用数据源
  close: true  #关闭集群心跳,此时集群认为所有数据源都是可用的,可以通过mycat提供的外部接口设置数据源可用信息达到相同效果
  clusters: [
  {name: repli ,
   replicaType: MASTER_SLAVE , # SINGLE_NODE:单一节点 ,MASTER_SLAVE:普通主从 GARELA_CLUSTER:garela cluster
   switchType: SWITCH , #NOT_SWITCH:不进行主从切换,SWITCH:进行主从切换
   readBalanceType: BALANCE_ALL  , #对于查询请求的负载均衡类型
   readBalanceName: BalanceRoundRobin , #对于查询请求的负载均衡类型
   writeBalanceName: BalanceRoundRobin ,  #对于修改请求的负载均衡类型
   masters:[defaultDs , defaultDs2], #主节点列表,普通主从,当主失去连接后,依次选择列表中存活的作为主节点
   replicas:[ defaultDs2],#从节点列表
   maxCon:, #集群最占用大连接限制
   heartbeat:{maxRetry: 3, #心跳重试次数
              minSwitchTimeInterval: 120000 , #最小主从切换间隔
              heartbeatTimeout: 100000 , #心跳超时值,毫秒
              slaveThreshold: 0 , # mysql binlog延迟值
              requestType: 'mysql' #进行心跳的方式,mysql或者jdbc两种
   }}
  ]
  timer: {initialDelay: 0, period: 1, timeUnit: SECONDS} #心跳定时器 initialDelay一般为0,mycat会在开启集群心跳,一个initialDelay+1秒之后开启服务器端口
#负载均衡类型 BALANCE_ALL:所有数据源参与负载均衡 BALANCE_ALL_READ:所以非master数据源参与负载均衡 BALANCE_NONE:只有master(一个)参与负载
plug:
  sequence:
    sequences: [
    {name: 'db1_travelrecord', clazz: io.mycat.plug.sequence.SequenceMySQLGenerator ,args: "sql : SELECT db1.mycat_seq_nextval('GLOBAL') , targetName:defaultDs"},
    {name: 'db1_address', clazz: io.mycat.plug.sequence.SequenceSnowflakeGenerator ,args: 'workerId:1'},
    ]
server:
  ip: 0.0.0.0
  port: 8066
  reactorNumber: 1
properties:
  key: value

#lib start
#lib end
```

### mycat安装包默认配置文件

schema.xml(是逻辑库定义和表以及分片定义的配置文件):
```xml
<?xml version="1.0"?>
<!DOCTYPE mycat:schema SYSTEM "schema.dtd">
<mycat:schema xmlns:mycat="http://io.mycat/">

	<schema name="TESTDB" checkSQLschema="true" sqlMaxLimit="100" randomDataNode="dn1">
		<!-- auto sharding by id (long) -->
		<!--splitTableNames 启用<table name 属性使用逗号分割配置多个表,即多个表使用这个配置-->
<!--fetchStoreNodeByJdbc 启用ER表使用JDBC方式获取DataNode-->
		<table name="customer" primaryKey="id" dataNode="dn1,dn2" rule="sharding-by-intfile" autoIncrement="true" fetchStoreNodeByJdbc="true">
			<childTable name="customer_addr" primaryKey="id" joinKey="customer_id" parentKey="id"> </childTable>
		</table>
		<!-- <table name="oc_call" primaryKey="ID" dataNode="dn1$0-743" rule="latest-month-calldate"
			/> -->
	</schema>
	<!-- <dataNode name="dn1$0-743" dataHost="localhost1" database="db$0-743"
		/> -->
	<dataNode name="dn1" dataHost="localhost1" database="db1" />
	<dataNode name="dn2" dataHost="localhost1" database="db2" />
	<dataNode name="dn3" dataHost="localhost1" database="db3" />
	<!--<dataNode name="dn4" dataHost="sequoiadb1" database="SAMPLE" />
	 <dataNode name="jdbc_dn1" dataHost="jdbchost" database="db1" />
	<dataNode	name="jdbc_dn2" dataHost="jdbchost" database="db2" />
	<dataNode name="jdbc_dn3" 	dataHost="jdbchost" database="db3" /> -->
	<dataHost name="localhost1" maxCon="1000" minCon="10" balance="0"
			  writeType="0" dbType="mysql" dbDriver="jdbc" switchType="1"  slaveThreshold="100">
		<heartbeat>select user()</heartbeat>
		<!-- can have multi write hosts -->
		<writeHost host="hostM1" url="jdbc:mysql://localhost:3306" user="root"
				   password="root">
		</writeHost>
		<!-- <writeHost host="hostM2" url="localhost:3316" user="root" password="123456"/> -->
	</dataHost>
	<!--
		<dataHost name="sequoiadb1" maxCon="1000" minCon="1" balance="0" dbType="sequoiadb" dbDriver="jdbc">
		<heartbeat> 		</heartbeat>
		 <writeHost host="hostM1" url="sequoiadb://1426587161.dbaas.sequoialab.net:11920/SAMPLE" user="jifeng" 	password="jifeng"></writeHost>
		 </dataHost>

	  <dataHost name="oracle1" maxCon="1000" minCon="1" balance="0" writeType="0" 	dbType="oracle" dbDriver="jdbc"> <heartbeat>select 1 from dual</heartbeat>
		<connectionInitSql>alter session set nls_date_format='yyyy-mm-dd hh24:mi:ss'</connectionInitSql>
		<writeHost host="hostM1" url="jdbc:oracle:thin:@127.0.0.1:1521:nange" user="base" 	password="123456" > </writeHost> </dataHost>

		<dataHost name="jdbchost" maxCon="1000" 	minCon="1" balance="0" writeType="0" dbType="mongodb" dbDriver="jdbc">
		<heartbeat>select 	user()</heartbeat>
		<writeHost host="hostM" url="mongodb://192.168.0.99/test" user="admin" password="123456" ></writeHost> </dataHost>

		<dataHost name="sparksql" maxCon="1000" minCon="1" balance="0" dbType="spark" dbDriver="jdbc">
		<heartbeat> </heartbeat>
		 <writeHost host="hostM1" url="jdbc:hive2://feng01:10000" user="jifeng" 	password="jifeng"></writeHost> </dataHost> -->

	<!-- <dataHost name="jdbchost" maxCon="1000" minCon="10" balance="0" dbType="mysql"
		dbDriver="jdbc"> <heartbeat>select user()</heartbeat> <writeHost host="hostM1"
		url="jdbc:mysql://localhost:3306" user="root" password="123456"> </writeHost>
		</dataHost> -->
</mycat:schema>
```

server.xml(是Mycat服务器参数调整和用户授权的配置文件)：
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- - - Licensed under the Apache License, Version 2.0 (the "License"); 
	- you may not use this file except in compliance with the License. - You 
	may obtain a copy of the License at - - http://www.apache.org/licenses/LICENSE-2.0 
	- - Unless required by applicable law or agreed to in writing, software - 
	distributed under the License is distributed on an "AS IS" BASIS, - WITHOUT 
	WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. - See the 
	License for the specific language governing permissions and - limitations 
	under the License. -->
<!DOCTYPE mycat:server SYSTEM "server.dtd">
<mycat:server xmlns:mycat="http://io.mycat/">
	<system>
	<property name="nonePasswordLogin">0</property> <!-- 0为需要密码登陆、1为不需要密码登陆 ,默认为0，设置为1则需要指定默认账户-->
	<property name="ignoreUnknownCommand">0</property><!-- 0遇上没有实现的报文(Unknown command:),就会报错、1为忽略该报文，返回ok报文。
	在某些mysql客户端存在客户端已经登录的时候还会继续发送登录报文,mycat会报错,该设置可以绕过这个错误-->
	<property name="useHandshakeV10">1</property>
    <property name="removeGraveAccent">1</property>
	<property name="useSqlStat">0</property>  <!-- 1为开启实时统计、0为关闭 -->
	<property name="useGlobleTableCheck">0</property>  <!-- 1为开启全加班一致性检测、0为关闭 -->
	<property name="sqlExecuteTimeout">300</property>  <!-- SQL 执行超时 单位:秒-->
		<property name="sequenceHandlerType">1</property>
	<!--<property name="sequnceHandlerPattern">(?:(\s*next\s+value\s+for\s*MYCATSEQ_(\w+))(,|\)|\s)*)+</property>
	INSERT INTO `travelrecord` (`id`,user_id) VALUES ('next value for MYCATSEQ_GLOBAL',"xxx");
	-->
	<!--必须带有MYCATSEQ_或者 mycatseq_进入序列匹配流程 注意MYCATSEQ_有空格的情况-->
	<property name="sequnceHandlerPattern">(?:(\s*next\s+value\s+for\s*MYCATSEQ_(\w+))(,|\)|\s)*)+</property>
	<property name="subqueryRelationshipCheck">false</property> <!-- 子查询中存在关联查询的情况下,检查关联字段中是否有分片字段 .默认 false -->
	<property name="sequenceHanlderClass">io.mycat.route.sequence.handler.HttpIncrSequenceHandler</property>
      <!--  <property name="useCompression">1</property>--> <!--1为开启mysql压缩协议-->
        <!--  <property name="fakeMySQLVersion">5.6.20</property>--> <!--设置模拟的MySQL版本号-->
	<!-- <property name="processorBufferChunk">40960</property> -->
	<!-- 
	<property name="processors">1</property> 
	<property name="processorExecutor">32</property> 
	 -->
        <!--默认为type 0: DirectByteBufferPool | type 1 ByteBufferArena | type 2 NettyBufferPool -->
		<property name="processorBufferPoolType">0</property>
		<!--默认是65535 64K 用于sql解析时最大文本长度 -->
		<!--<property name="maxStringLiteralLength">65535</property>-->
		<!--<property name="sequenceHandlerType">0</property>-->
		<!--<property name="backSocketNoDelay">1</property>-->
		<!--<property name="frontSocketNoDelay">1</property>-->
		<!--<property name="processorExecutor">16</property>-->
		<!--
			<property name="serverPort">8066</property> <property name="managerPort">9066</property> 
			<property name="idleTimeout">300000</property> <property name="bindIp">0.0.0.0</property>
			<property name="dataNodeIdleCheckPeriod">300000</property> 5 * 60 * 1000L; //连接空闲检查
			<property name="frontWriteQueueSize">4096</property> <property name="processors">32</property> -->
		<!--分布式事务开关，0为不过滤分布式事务，1为过滤分布式事务（如果分布式事务内只涉及全局表，则不过滤），2为不过滤分布式事务,但是记录分布式事务日志-->
		<property name="handleDistributedTransactions">0</property>
		
			<!--
			off heap for merge/order/group/limit      1开启   0关闭
		-->
		<property name="useOffHeapForMerge">0</property>

		<!--
			单位为m
		-->
        <property name="memoryPageSize">64k</property>

		<!--
			单位为k
		-->
		<property name="spillsFileBufferSize">1k</property>

		<property name="useStreamOutput">0</property>

		<!--
			单位为m
		-->
		<property name="systemReserveMemorySize">384m</property>


		<!--是否采用zookeeper协调切换  -->
		<property name="useZKSwitch">false</property>

		<!-- XA Recovery Log日志路径 -->
		<!--<property name="XARecoveryLogBaseDir">./</property>-->

		<!-- XA Recovery Log日志名称 -->
		<!--<property name="XARecoveryLogBaseName">tmlog</property>-->
		<!--如果为 true的话 严格遵守隔离级别,不会在仅仅只有select语句的时候在事务中切换连接-->
		<property name="strictTxIsolation">false</property>
		<!--如果为0的话,涉及多个DataNode的catlet任务不会跨线程执行-->
		<property name="parallExecute">0</property>
	</system>
	
	<!-- 全局SQL防火墙设置 -->
	<!--白名单可以使用通配符%或着*-->
	<!--例如<host host="127.0.0.*" user="root"/>-->
	<!--例如<host host="127.0.*" user="root"/>-->
	<!--例如<host host="127.*" user="root"/>-->
	<!--例如<host host="1*7.*" user="root"/>-->
	<!--这些配置情况下对于127.0.0.1都能以root账户登录-->
	<!--
	<firewall>
	   <whitehost>
	      <host host="1*7.0.0.*" user="root"/>
	   </whitehost>
       <blacklist check="false">
       </blacklist>
	</firewall>
	-->

	<user name="root" defaultAccount="true">
		<property name="password">123456</property>
		<property name="schemas">TESTDB</property>
		<property name="defaultSchema">TESTDB</property>
		<!--No MyCAT Database selected 错误前会尝试使用该schema作为schema，不设置则为null,报错 -->
		
		<!-- 表级 DML 权限设置 -->
		<!-- 		
		<privileges check="false">
			<schema name="TESTDB" dml="0110" >
				<table name="tb01" dml="0000"></table>
				<table name="tb02" dml="1111"></table>
			</schema>
		</privileges>		
		 -->
	</user>

	<user name="user">
		<property name="password">user</property>
		<property name="schemas">TESTDB</property>
		<property name="readOnly">true</property>
		<property name="defaultSchema">TESTDB</property>
	</user>

</mycat:server>

```