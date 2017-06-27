---
layout: post
title: Spring Profiles
---

Spring Profiles allow users to use various configurations, register different beans etc. Working on different environments may cause some problems or we need to run with quick setup on our development environment. To make it more clear, we can mention to database issues.

One of my colleague may change hibernate mappings. To get rid of schema compatibility problems, I want to create database schemas on every run at my development environment. When it comes to deployment releases, migration scripts will create database and schemas so I won’t need to create it programmatically.

For the sample case mentioned above, I create 2 different spring profile as following:

```xml
<beans profile="dev">
    <bean id="mySessionFactoryBean" class="org.springframework.orm.hibernate5.LocalSessionFactoryBean">
        <property name="dataSource" ref="myHSQLDataSourceBean"/>
        <property name="mappingLocations" value="classpath*:**/*.hbm.xml"/>
        <property name="hibernateProperties">
            <value>                
                hibernate.hbm2ddl.auto = create-drop                
            </value>
        </property>
    </bean>
</beans>
```

```xml
<beans profile="default">
    <bean id="mySessionFactoryBean" class="org.springframework.orm.hibernate5.LocalSessionFactoryBean">
        <property name="dataSource" ref="myHSQLDataSourceBean"/>
        <property name="mappingLocations" value="classpath*:**/*.hbm.xml"/>        
    </bean>
</beans>
```

If ```-Dspring.profiles.active=dev``` is given as JVM options, dev profile is loaded. In case of not specifiying active profile explicitly, default profile is loaded as default.

For further reading, please look at following links:

* http://docs.spring.io/autorepo/docs/spring-boot/current/reference/html/boot-features-profiles.html
* https://www.mkyong.com/spring/spring-profiles-example/
