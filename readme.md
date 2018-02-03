##JDBC是对java.sql中数据库相关操作的实现，特定的JDBC版本对应一些版本的JDK


------
##fabric : MySQL Fabric 功能相关类
######关于MySQL Fabric 功能： https://www.2cto.com/database/201408/327941.html
######MySQL Fabric部署：www.csdn.net/article/2014-08-20/2821300

>    *FabricCommunicationException :为MySQL >Fabric功能，client与MySQL通信异常类
>
>    *FabricConnection :fabric client通信
>
>    *FabricStateResponse :fabric状态响应，主要设置fabric数据生命期
>
>    *HashSharedMapping : fabric 功能相关，共享哈希映射，其范围大小由哈希的键值对决定
>
>    *RangeSharedMapping :fabric 功能相关，按照数据的范围，根据索引从高到低，划分数据；通过共享索引查找数据对应的主机。
>
>    *Response :对fabric request的response
>
>    *Server :数据库server
>
>    *ServerGroup :负责处理一组数据集的一组server
>
>    *ServerMode :server工作模式 lib/mysql/fabric/server.py
>
>    *ServerRole :enum ServerRole FAULTY, SPARE, SECONDARY, PRIMARY, CONFIGURING;
>
>    *ShardIndex :索引分片 映射数据段的 实际物理地址，数据段由key与bound的关系决定
>
>    *ShardingType :分片类型，数据行分布在各个server间
>
>    *ShardMapping :对一组数据分片的映射分片
>
>    *ShardMappingFactory :创建ShardMapping的工厂
>
>    *ShardTable :对某个表在分片间分配的描述

------
##fabric.hibernate :hibernate 4 的fabric 接口
######相关信息：http://docs.jboss.org/hibernate/orm/4.1/javadocs/org/hibernate/service/jdbc/connections/spi/MultiTenantConnectionProvider.html

------
##fabric.jdbc :Fabric访问数据库的接口
>    *ErrorReportingExceptionInterceptor :拦截错误报告中的异常
>
>    *FabricMySQLConnection :对fabric中数据库访问的控制，继承于JDBC中MySQLConnection
>
>    *FabricMySQLConnectionProperties :对FabricMySQLConnection进行额外配置
>
>    *FabricMySQLConnectionProxy : MySQL Fabric对MySQL servers集合的代理
>

------
##fabric.xmlrpc
####xml-rpc与apach：https://www.ibm.com/developerworks/cn/webservices/1211_zhusy_rpc/
>    *Client :基于xml的远程过程调用客户端

------
##fabric.xmlrpc.base:xml-rpc相关功能，是xml-rpc的实现
####xml-rpc协议规格说明：http://www.cnblogs.com/sitemanager/archive/2013/02/27/2935282.html
>    *Value :把所有数据库的数据的值及其类型（rpc数据类型，而不是java数据类型）封装为一个object（xml-rpc的value）
>
>    *Data :对value list的封装
>
>    *Array :对Data数组的一些处理（xml-rpc的value可以为数组，每个数组一个独立的data）
>
>    *Fault :对单个value的封装（xml-rpc的故障信息）
>
>    *Member :将一个name与value绑定封装
>
>    *MethodCall :将一个放法名与其输入参数做封装，来描述一个放法调用
>
>    *MethodResponse :将参数名与参数值输出到xml格式的配置文件
>
>    *Param :对value的封装，主要是作为参数（xml-prc的param）
>
>    *Params :对Param list的封装，参数列表（xml-rpc的params）
>
>    *ResponseParser :rpc响应解析
>
>    *Struct :把member list封装(xml-rpc的struct)
>


------
##fabric.xmlrpc.exceptions
>
>    *rpc异常处理
>

------
##JDBC
*AbandonedConnectionCleanupThread :一个用来清理无用connection的线程。
启动该线程会创建一个对connect队列的引用，来处理connection队列中的失效对象，当一个connection的引用被置于NULL时，调用NonRegisteringDriver类的cleanup放法清理连接，IO等资源


      	try {
                Reference<? extends ConnectionImpl> ref = NonRegisteringDriver.refQueue.remove(100);
                if (ref != null) {
                    try {
                        ((ConnectionPhantomReference) ref).cleanup();
                    } finally {
                        NonRegisteringDriver.connectionPhantomRefs.remove(ref);
                    }
                }
            } catch (Exception ex) {
                // no where to really log this if we're static
            }


这里jdbc 5.1没有log exceptions，可以根据需求补上去
<br>
*AssertionFailedException:断言异常处理
<br>
*AuthenticationPlugin:认证插件接口
<br>
*BalanceStrategy:平衡策略接口
<br>
*BestResponseTimeBalanceStrategy:BalanceStrategy的实现，jdbc5.1.29采用的平衡策略，优先选取响应时间最短的connection
<br>
*Blob:二进制大对象类（note：pointer指向SQL BLOB类的data）
<br>
*BlobFromLocator：结果集的BLOB
<br>
*Buffer：buffer类，SQL语句最终都被使用MySQL协议转化为buffer
<br>
*BufferRow：结果集中的一行
<br>
*ByteArrayRow：对结果集数据行缓存
<br>
*CacheAdapter：缓存适配器接口
<br>
*CacheAdapterFactory:适配器接口工厂
<br>
*CachedResultSetMetaData：主要用来保存所有缓存的结果集的metadata，用map来存储列号和列名
，<br>
*CallableStatement:JDBC过程存储，创建存储过程，批量执行sql
<br>
*CharsetMapping:mysql字符集和java字符集之间的映射
<br>
*Clob:字符大对象，存储为数据库表的某一行的一个列值
<br>
*CommunicationsException:处理与数据库的通信异常
<br>
*CompressedInputStream:解压缩mysql端来的压缩数据（mysql传输协议启用压缩功能时启用，MYSQL的C/S通信使用的是MYSQL协议，之后再详细解析）
<br>
<br>
!Connection :一个接口类，继承于java.sql.Connection,由ConnectionImpl实现
>note:因为依赖于JDK版本，不同版本build时可能出现error，这时候参考你使用的版本的JDK中的sql.Connection，对该接口部分重写，对ConnectionImpl同理
>在jdbc 5.1之前的版本没有ConnectionImpl，直接在此类中实现

!ConnectionImpl :一个Connection就是一个app到db的会话，包含了SQL语句，结果集，metadata等
>主要方法及参数：

	ConnectionImpl(String hostToConnectTo, int portToConnectTo, Properties info, String databaseToConnectTo, String url)

	//主机名，端口号，【用户名，密码】，数据库名，url
<br>
*ConnectionFeatureNotAvailable:当用户请求链接的特性时，使用的JDBC版本却不支持此功能，抛出该异常
<br>
*ConnectionProperties:接口，以XML文档的形式，取得connection的配置信息
<br>
*Constants:JDBC中用到的一些常量
<br>
*DatabaseMetaData:取得数据库或者结果集的元数据
<br>
*DatabaseMetaDataUsingInfoSchema：在mysql 5.0以上支持，元数据的information schema（数据库配置信息，表信息，列名，数据类型，权限等等）
<br>
*Driver：jdbc中允许多个数据库驱动，jdbc的driver manager启动时候会加载所有驱动，手动加载驱动器Driver.forName(""),当请求建立链接时，Driver Manager会从drivers（队列）中找出合适的实例来链接
<br>
*EscapeProcessor：对SQL语句进行转义处理（数据类型，起始符等）
<br>
*EscapeProcessorResult：接收SQL的转义结果，防止一个SQL被多次转义
<br>
*EscapeTokenizer：把SQL语言切割成SQL片段和转义符片段
<br>
*ExportControlled：把mysqlio中的socke链接转为sslsocket
<br>
*Extension：数据库driver的扩展插件接口
<br>
*
<br>
<br>
<br>
<br>