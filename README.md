# kafka-connect
执行如下命令启动connect
sudo ./connect-distributed.sh ../config/connect-distributed.properties ../config/connect-mysql-source.properties

注意：如果启动有异常 Caused by: java.lang.ClassNotFoundException: javax.xml.bind.Validation
  引入下面依赖即可
      jaxb-api maven地址：http://mvnrepository.com/artifact/javax.xml.bind/jaxb-api
      jaxb-impl maven地址：http://mvnrepository.com/artifact/com.sun.xml.bind/jaxb-impl
      jaxb-core maven地址：http://mvnrepository.com/artifact/com.sun.xml.bind/jaxb-core
      activation maven地址：http://mvnrepository.com/artifact/javax.activation/activation
  将jar包复制到 /kafka_2.12-1.0.0/libs 下即可
    
