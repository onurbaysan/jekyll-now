---
layout: post
title: Creating List of Beans in Spring
---


Let’s think we need a list of IMoveable in a class and I need to pass it through property injection in Spring. The easiest way is: (shown below).

```xml
<bean id="sceneBean" class="com.onurbaysan.Scene">        
    <property name="moveableObjectList">
        <list>
            <ref bean="cubeMoveableObjectBean" />
            <ref bean="sphereMoveableObjectBean" />
        </list>
    </property>
</bean>
```

It seems fine if you have a few items and new items won’t be added. What if you have more items (eg 100)? What if another developer add pyramidMoveableObject and forget to add it into moveableObjectList?

In these scenarios, we have prettier way to do it. I a class to find all beans which implements IMoveable interfaces, more generally find beans of given types.

I’ll use 2 interface in my class:

1. [FactoryBean](https://spring.io/blog/2011/08/09/what-s-a-factorybean) inteface of Spring, to create list of items
2. [ApplicationContextAware](http://docs.spring.io/autorepo/docs/spring/3.2.5.RELEASE/javadoc-api/org/springframework/context/ApplicationContextAware.html) interface to access beans in my context.

```java
public class SpringListFactory
			implements FactoryBean, ApplicationContextAware
{
    private ApplicationContext applicationContext;
    private Class classType;

    @Override
    public Object getObject() throws Exception
    {
        List result = new ArrayList();
        result.addAll(applicationContext.getBeansOfType(classType).values());
        return result;
    }

    @Override
    public Class<?> getObjectType()
    {
        return List.class;
    }

    @Override
    public boolean isSingleton()
    {
        return false;
    }

    @Override
    public void setApplicationContext( ApplicationContext applicationContext )
    {
        this.applicationContext = applicationContext;
    }

    public void setClassType( Class classType )
    {
        this.classType = classType;
    }
}
```

Then we can create our IMoveable list through the following beans:

```xml
<bean id="moveableObjectListBean" class="com.onurbaysan.SpringListFactory">
    <property name="classType" value="com.onurbaysan.IMoveable" />
</bean>
```

At the end, our scene bean looks like this:

```xml
<bean id="sceneBean" class="com.onurbaysan.Scene">      
    <property name="moveableObjectList" ref="moveableObjectListBean" />   
</bean>
```
