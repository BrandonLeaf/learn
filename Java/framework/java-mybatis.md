# Java - MyBatis
更新时间：2018.07.10

目录
---


## 初始化配置

使用SqlSessionFactoryBean初始化MyBatis配置文件

```java
 @Bean(name = "sqlSessionFactory")
    public SqlSessionFactoryBean createSqlSessionFactoryBean() {
        SqlSessionFactoryBean sqlSessionFactoryBean = new SqlSessionFactoryBean();
        sqlSessionFactoryBean.setDataSource(dataSource);
        org.apache.ibatis.session.Configuration configuration=new org.apache.ibatis.session.Configuration();
        //使用jdbc的getGeneratedKeys获取数据库自增主键值
        configuration.setUseGeneratedKeys(false);
        //使用列别名替换列名 select user as User
        configuration.setUseColumnLabel(true);
        //-自动使用驼峰命名属性映射字段   userId    user_id
        configuration.setMapUnderscoreToCamelCase(true);
        configuration.setCallSettersOnNulls(true);
        sqlSessionFactoryBean.setConfiguration(configuration);
        sqlSessionFactoryBean.setFailFast(true);
        return sqlSessionFactoryBean;
    }

```


## 常见异常

* invalid character

    * 检查是否多了分号“；”