# Beetl
更新时间：20180731

目录
---
<!-- TOC depthFrom:2 updateOnSave:true -->

- [BeetlSQL](#beetlsql)
    - [参考链接](#参考链接)
    - [Database Config](#database-config)
        - [Spring boot 配置主从数据源](#spring-boot-配置主从数据源)

<!-- /TOC -->

---

## BeetlSQL

### 参考链接

* [BeetlSQL API](http://ibeetl.com/guide/#beetlsql)
* [Spring Boot - Configure Two DataSources](https://docs.spring.io/spring-boot/docs/2.0.4.RELEASE/reference/htmlsingle/#howto-two-datasources)

### Database Config

#### Spring boot 配置主从数据源

spring boot yml 配置文件

```yml
spring:
  datasource:
    master:
      name: masterDatasource
      type: com.zaxxer.hikari.HikariDataSource
      #最大连接数
      maximum-pool-size: 30
      #一个连接的生命时长（毫秒），超时而且没被使用则被释放（retired），缺省:30分钟，建议设置比数据库超时时长少30秒，参考MySQL wait_timeout参数（show variables like '%timeout%';）
      max-lifetime: 1200000
      #一个连接idle状态的最大时长（毫秒），超时则被释放（retired），缺省:10分钟
      idle-timeout: 600000
      #等待连接池分配连接的最大时长（毫秒），超过这个时长还没可用的连接则发生SQLException， 缺省:30秒
      connection-timeout: 30000
      #连接只读数据库时配置为true， 保证安全
      read-only: false
      url: jdbc:mysql://127.0.0.1:3306/TEST_MASTER_DB?characterEncoding=utf8&useSSL=false
      username: app
      password: A123123.
    slave:
      name: slaveDatasource
      type: com.zaxxer.hikari.HikariDataSource
      #最大连接数
      maximum-pool-size: 30
      #一个连接的生命时长（毫秒），超时而且没被使用则被释放（retired），缺省:30分钟，建议设置比数据库超时时长少30秒，参考MySQL wait_timeout参数（show variables like '%timeout%';）
      max-lifetime: 1200000
      #一个连接idle状态的最大时长（毫秒），超时则被释放（retired），缺省:10分钟
      idle-timeout: 600000
      #等待连接池分配连接的最大时长（毫秒），超过这个时长还没可用的连接则发生SQLException， 缺省:30秒
      connection-timeout: 30000
      #连接只读数据库时配置为true， 保证安全
      read-only: true
      url: jdbc:mysql://10.182.95.102:3306/TEST_SLAVE_DB?characterEncoding=utf8&useSSL=false
      username: app
      password: A123123.

  devtools:
    restart:
      #热部署
      enabled: true

#模板引擎
beetl:
  #启用/禁用
  enabled: false

#持久层
beetlsql:
  #启用/禁用
  enabled: true
  #名字转换规则--驼峰
  nameConversion: org.beetl.sql.core.UnderlinedNameConversion
  daoSuffix: Dao
  basePackage: com.cpicdg
  dbStyle: org.beetl.sql.core.db.MySqlStyle
  sqlPath: /sql

#SQL变动监听
beetl-beetlsql:
  dev: true
```

DataSourceConfig.java
```java
import com.ibeetl.starter.BeetlSqlCustomize;
import org.beetl.sql.core.*;
import org.beetl.sql.ext.DebugInterceptor;
import org.beetl.sql.ext.TimeStatInterceptor;
import org.beetl.sql.ext.spring4.BeetlSqlDataSource;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.boot.autoconfigure.jdbc.DataSourceProperties;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Primary;

import javax.sql.DataSource;

/**
 * @Author cpicapp
 * @Date create in 3:19 PM 7/28/18
 * @Version 1.0
 * Description:
 * 数据源配置
 */
@Configuration
public class DataSourceConfig {

    /**
     * 读取主数据源配置
     */
    @Bean(value = "masterParams")
    @Primary
    @ConfigurationProperties("spring.datasource.master")
    public DataSourceProperties masterDataSourceProperties() {
        return new DataSourceProperties();
    }

    /**
     * 读取从数据源配置
     */
    @Bean(value = "slaveParams")
    @ConfigurationProperties("spring.datasource.slave")
    public DataSourceProperties slaveDataSourceProperties() {
        return new DataSourceProperties();
    }

    /**
     * 初始化主数据源，注入连接池配置
     */
    @Bean(name = "masterDataSource")
    @Primary
    @ConfigurationProperties("spring.datasource.master")
    public DataSource masterDataSource(@Qualifier("masterParams")DataSourceProperties dataSourceProperties) {
        return dataSourceProperties.initializeDataSourceBuilder().build();
    }

    /**
     * 初始化从数据源，注入连接池配置
     */
    @Bean(name = "slaveDataSource")
    @ConfigurationProperties("spring.datasource.slave")
    public DataSource slaveDataSource(@Qualifier("slaveParams")DataSourceProperties dataSourceProperties) {
        return dataSourceProperties.initializeDataSourceBuilder().build();
    }

    /**
     * Beetl 数据源
     * @param master 主数据源
     * @param slave 从数据源
     */
    @Bean
    public BeetlSqlDataSource beetlSqlDataSource(
            @Qualifier("masterDataSource") DataSource master
            , @Qualifier("slaveDataSource") DataSource slave) {
        BeetlSqlDataSource source = new BeetlSqlDataSource();
        source.setMasterSource(master);
        source.setSlaves(new DataSource[]{slave});
        return source;
    }

    /**
     * beetl 自定义 SQL 监听器
     */
    //@Bean
    public BeetlSqlCustomize beetlSqlCustomize() {
        return sqlManagerFactoryBean -> {
            Interceptor[] interceptor = {
                    new DebugInterceptor(),
                    new TimeStatInterceptor(3000)
            };
            sqlManagerFactoryBean.setInterceptors(interceptor);
        };
    }
}

```