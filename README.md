# kafka-connect
执行如下命令启动connect
sudo ./connect-distributed.sh ../config/connect-distributed.properties 【../config/connect-mysql-source.properties】

注意：如果启动有异常 Caused by: java.lang.ClassNotFoundException: javax.xml.bind.Validation
  引入下面依赖即可
      jaxb-api maven地址：http://mvnrepository.com/artifact/javax.xml.bind/jaxb-api
      jaxb-impl maven地址：http://mvnrepository.com/artifact/com.sun.xml.bind/jaxb-impl
      jaxb-core maven地址：http://mvnrepository.com/artifact/com.sun.xml.bind/jaxb-core
      activation maven地址：http://mvnrepository.com/artifact/javax.activation/activation
  将jar包复制到 /kafka_2.12-1.0.0/libs 下即可
  
  
==============================横线以下是api调用实例====================
  
创建JdbcSourceConnector-connector 
	url:http://localhost:8083/connectors 
	method:POST
	param:
			{
				"name":"mysql-source",
				"config":{
							"connector.class":"io.confluent.connect.jdbc.JdbcSourceConnector",
							"tasks.max":1,
							"connection.url":"jdbc:mysql://localhost:3306/jpatest?user=root&password=000",
							"mode":"incrementing",
							"incrementing.column.name":"id",
							"topic.prefix":"mysql.",
							"topics":"t_resources"
				}

			}


查看
	url:http://localhost:8083/connectors
	method:GET

============================================================


分布式 REST API 
	由于Kafka Connect的目的是作为一个服务运行，提供了一个用于管理connector的REST API。默认情况下，此服务的端口是8083。以下是当前支持的终端入口：

	GET /connectors - 返回活跃的connector列表
	POST /connectors - 创建一个新的connector；请求的主体是一个包含字符串name字段和对象config字段（connector的配置参数）的JSON对象。
	GET /connectors/{name} - 获取指定connector的信息
	GET /connectors/{name}/config - 获取指定connector的配置参数
	PUT /connectors/{name}/config - 更新指定connector的配置参数
	GET /connectors/{name}/status - 获取connector的当前状态，包括它是否正在运行，失败，暂停等。
	GET /connectors/{name}/tasks - 获取当前正在运行的connector的任务列表。
	GET /connectors/{name}/tasks/{taskid}/status - 获取任务的当前状态，包括是否是运行中的，失败的，暂停的等，
	PUT /connectors/{name}/pause - 暂停连接器和它的任务，停止消息处理，直到connector恢复。
	PUT /connectors/{name}/resume - 恢复暂停的connector（如果connector没有暂停，则什么都不做）
	POST /connectors/{name}/restart - 重启connector（connector已故障）
	POST /connectors/{name}/tasks/{taskId}/restart - 重启单个任务 (通常这个任务已失败)
	DELETE /connectors/{name} - 删除connector, 停止所有的任务并删除其配置
	Kafka Connector还提供了获取有关connector plugins信息的REST API：

	GET /connector-plugins- 返回已在Kafka Connect集群安装的connector plugin列表。请注意，API仅验证处理请求的worker的connector。这以为着你可能看不不一致的结果，特别是在滚动升级的时候（添加新的connector jar）

	PUT /connector-plugins/{connector-type}/config/validate - 对提供的配置值进行验证，执行对每个配置验证，返回验证的建议值和错误信息。

