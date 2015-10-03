---
layout: post
title:  "ActiveMQ Properties File"
date:   2009-05-07 01:09:00
categories: activemq
---
When configuring ActiveMQ most developers are relieved to find that they use a Spring style of configuration which they are used to. This gives a lot of flexibility when writing your config file but the one thing I was looking for I couldn't find. How do I include a properties file with my deployment of ActiveMQ? I couldn't find any examples on the ActiveMQ website but remember that this is Spring and one of the first couple lines should give you a hint. Here is the beginning of the default ActiveMQ configuration:
{% highlight xml %}
<beans
  xmlns="http://www.springframework.org/schema/beans"
  xmlns:amq="http://activemq.apache.org/schema/core"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-2.0.xsd
  http://activemq.apache.org/schema/core http://activemq.apache.org/schema/core/activemq-core.xsd
  http://camel.apache.org/schema/spring http://camel.apache.org/schema/spring/camel-spring.xsd">
    <!-- Allows us to use system properties as variables in this configuration file -->
    <bean class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer"/>
    <broker xmlns="http://activemq.apache.org/schema/core" brokerName="localhost" dataDirectory="${activemq.base/data}">
        <!-- Destination specific policies using destination names or wildcards -->
        <destinationPolicy>
            <policyMap>
                <policyEntries>
                    <policyEntry queue=">" memoryLimit="5mb"/>
                    <policyEntry topic=">" memoryLimit="5mb">
                      <!-- you can add other policies too such as these
                        <dispatchPolicy>
                            <strictOrderDispatchPolicy/>
                        </dispatchPolicy>
                        <subscriptionRecoveryPolicy>
                            <lastImageSubscriptionRecoveryPolicy/>
                        </subscriptionRecoveryPolicy>
                      -->
                    </policyEntry>
                </policyEntries>
            </policyMap>
        </destinationPolicy>
{% endhighlight %}
We can use the Spring class PropertyPlaceholderConfigurer to point to a properties file, myConfig.properties, location. The result would look like this: 
{% highlight xml %}
<!-- Allows us to use system properties as variables in this configuration file -->
    <bean class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer">
         <property name="locations" value="classpath:myCOnfig.properties"/>
    </bean>
{% endhighlight %}
Now any property you have in the file can be referenced in the ActiveMQ config with the standard Spring property exressions. Ex: ```${thisisaproperty}```.

This is handy for deploying a message broker out to different environments, quickly changing port numbers, database connections, or memory settings.
