# Description and documentation of Java Melody : monitoring of Java EE applications #

## Table of contents ##
  * [Introduction](#Introduction.md)
  * [Overhead](#Overhead.md)
  * [Jenkins Plugin](#Jenkins_Plugin.md)
  * [Atlassian JIRA, Confluence and Bamboo Plugin](#Atlassian_JIRA,_Confluence_and_Bamboo_Plugin.md)
  * [Liferay Plugin](#Liferay_Plugin.md)
  * [Sonar Plugin](#Sonar_Plugin.md)
  * [Grails Plugin](#Grails_Plugin.md)
  * [JavaMelody Setup](#Setup.md)
    * [1. Jar files](#1._Jar_files.md)
    * [2. web.xml file](#2._web.xml_file.md)
    * [3. First results](#3._First_results.md)
    * [4. PDF report generation](#4._PDF_report_generation.md)
    * [5. Supplements in web.xml](#5._Supplements_in_web.xml.md)
    * [6. Optional parameters](#6._Optional_parameters.md)
    * [7. JDBC](#7._JDBC.md)
    * [8. Business facades (ejb-jar.xml file if EJB3)](#8._Business_facades_(ejb-jar.xml_file_if_EJB3).md)
    * [9. Business facades (if Spring)](#9._Business_facades_(if_Spring).md)
    * [10. Business facades (if Guice)](#10._Business_facades_(if_Guice).md)
    * [11. JSF Actions](#11._JSF_Actions.md)
    * [12. Struts 2 Actions](#12._Struts_2_Actions.md)
    * [13. Batch jobs (if Quartz)](#13._Batch_jobs_(if_Quartz).md)
    * [14. Weekly, daily or monthly reports by mail](#14._Weekly,_daily_or_monthly_reports_by_mail.md)
    * [15. Database user](#15._Database_user.md)
    * [16. Security](#16._Security.md)
    * [17. SecurityManager](#17..md)
    * [18. Full results](#18._Full_results.md)
  * [Dependencies](#Dependencies.md)
  * [Advanced documentation (setup with an ear, centralized collect server, monitoring of sql requests in GlassFish v3, JBoss AS 7, JonAS 5, debugging logs...)](#Advanced_documentation_(setup_with_an_ear,_centralized_collect_s.md)
  * [Compilation and development](#Compilation_and_development.md)

## Introduction ##

The goal of JavaMelody is to monitor Java or Java EE application servers in QA and production environments.

It is not a tool to simulate requests from users, it is a tool to measure and calculate statistics on real operation of an application depending on the usage of the application by users.

JavaMelody is mainly based on statistics of requests and on evolution charts.

It allows to improve applications in QA and production and helps to:

  * give facts about the average response times and number of executions

  * make decisions when trends are bad, before problems become too serious

  * optimize based on the more limiting response times

  * find the root causes of response times

  * verify the real improvement after optimizations

License: ASL since v1.50, LGPL before v1.50

URL: http://javamelody.googlecode.com/

Java version required for execution:

  * 1.6 or later

  * JDK or JRE from Sun or JRockit from Oracle/BEA or J9 from IBM

Server version required for execution: servlet api 2.4 at least (or JavaEE 1.4),
like Tomcat 5.5, 6 or 7, GlassFish v2 or v3, JBoss 4, 5, 6 or 7, Jonas 4 or 5, Jetty 6 or 7, WebLogic 9, 10 or 11

Required dependency: JRobin (LGPL) for evolution charts

Optional dependencies: iText v2.1.x (LGPL or MPL) for reports in pdf format in addition to html, Spring AOP, AOP alliance, Spring core, Spring beans and Commons logging for monitoring of Spring beans, Ehcache for monitoring of caches, Quartz for monitoring of batch jobs

Language: English, French, German, Portuguese and Chinese

Browser: The html report of JavaMelody is best viewed with
[Firefox](http://www.mozilla.com),
[Chrome](http://www.google.com/chrome)
or MSIE9 (MSIE7 not recommended).

[Online help](http://javamelody.googlecode.com/svn/trunk/javamelody-core/src/site/resources/Online_help_of_the_monitoring.pdf)

[Roots conference slides by David Karlsen](http://www.slideshare.net/djkarlsen/significance-of-metrics)


## Overhead ##

Some discussions on the monitoring [overhead in production](Overhead.md) were archived for information.

## Jenkins Plugin ##

> If you want to monitor a [Jenkins](http://jenkins-ci.org/) server (or a [Hudson](http://hudson-ci.org/)) server, you can install JavaMelody by pointing and clicking with a [Jenkins plugin](http://wiki.jenkins-ci.org/display/Jenkins/Monitoring). For this, open Jenkins and click "Manage Jenkins", "Manage plugins", "Available" then check the plugin called "Monitoring" and click the "Install" button at the bottom of the page. Restart Jenkins after the installation of the plugin. You can then open the report from the "Manage Jenkins" page or from the address `http://yourhost/monitoring`. You can also open the report for the nodes from the address `http://yourhost/monitoring/nodes`. That's it.

## Atlassian JIRA, Confluence and Bamboo Plugin ##

> If you want to monitor a [JIRA](http://www.atlassian.com/software/jira/),
> [Confluence](http://www.atlassian.com/software/confluence/) or a [Bamboo](http://www.atlassian.com/software/bamboo/) server, you can install JavaMelody easily with a JIRA/Confluence/Bamboo plugin. For this, see the following [page](AtlassianPlugin.md)

## Liferay Plugin ##

> To monitor a [Liferay](http://www.liferay.com) server v6.1 or later, you can install a [Liferay javamelody plugin](https://github.com/evernat/liferay-javamelody):
    * Download the latest Liferay plugin file from [releases](https://github.com/evernat/liferay-javamelody/releases)
    * Copy the file into the "deploy" directory of your Liferay server and wait a few seconds
    * Liferay automatically deploys the file and removes it from "deploy"
> That's it!

> You can browse the monitoring page at `http://<host>:<port>/monitoring`

> For example, if the server is local: `http://localhost:8080/monitoring`

> Authentication and portal "Administrator" role are required to access this monitoring page.

> Known [issues](https://github.com/evernat/liferay-javamelody/issues): The SQL requests are not monitored. And pull requests are welcome.

## Sonar Plugin ##

> To monitor a [Sonar](http://www.sonarqube.org/) server v3.1 or later, you can install a [Sonar javamelody plugin](https://github.com/evernat/sonar-javamelody):
    * Download the latest Sonar plugin jar file from [releases](https://github.com/evernat/sonar-javamelody/releases)
    * Copy the file into the "extensions/plugins" directory of your Sonar server
    * Restart Sonar

> You can browse the monitoring page at `http://<host>:<port>/monitoring`

> For example, if the server is local: `http://localhost:9000/monitoring`

> Known [issues](https://github.com/evernat/sonar-javamelody/issues): There is currently no authentication to secure the access to the monitoring page. The SQL requests are not monitored. And pull requests are welcome.

## Grails Plugin ##

> If you want to monitor an application developed with [Grails](http://www.grails.org), you can install JavaMelody in just one command with a Grails plugin: [JavaMelody Grails plugin](http://www.grails.org/plugin/grails-melody).

> `grails install-plugin grails-melody`

## JavaMelody Setup ##

> An important value of the monitoring is a very simple and fast installation process.
> And in general, integration is done by the software provider without any intervention from the client.

> This integration can be done in less than 10 minutes, by automatic discovery of environment:
> it only requires to copy 2 jar files and to add 10 lines in a xml file.

> Then this integration can be completed later by configuration as needed.

> If you deploy your application with a war file or with an equivalent directory, read the following chapters.

> But if you deploy your application with an ear file, probably because you use EJBs, follow the [UserGuideAdvanced](UserGuideAdvanced.md).

> Please note that the javamelody.war file is not used in installation here. And the javamelody.war file is **not needed** in most use cases.

### 1. Jar files ###

> Copy the files [javamelody.jar](https://github.com/javamelody/javamelody/releases) and [jrobin-x.jar](http://javamelody.googlecode.com/files/jrobin-1.5.9.jar), located at the root of the supplied [javamelody.zip](https://github.com/javamelody/javamelody/releases) file,
> to the WEB-INF/lib directory of the war of the webapp to monitor.

> Or if you use Maven, add the javamelody-core [dependency](UserGuide#Dependencies.md) in the pom.xml file of your webapp.

### 2. `web.xml` file ###

> If your application server is compatible with Servlet API 3.0 (like tomcat 7, glassfish v3 or jboss 6),
> this paragraph is generally not needed, you can skip it and launch the server as in the next paragraph,
> except if you use a web.xml file without version="3.0".
> Otherwise add the following lines in the file `WEB-INF/web.xml` of the war of the webapp,
> before the description of your servlet :

```
	<filter>
		<filter-name>javamelody</filter-name>
		<filter-class>net.bull.javamelody.MonitoringFilter</filter-class>
	</filter>
	<filter-mapping>
		<filter-name>javamelody</filter-name>
		<url-pattern>/*</url-pattern>
	</filter-mapping>
	<listener>
		<listener-class>net.bull.javamelody.SessionListener</listener-class>
	</listener>
```

> If you add these lines in a Servlet API 3.0 webapp, you may also add `<async-supported>true</async-supported>` in the filter to support asynchronous requests.

### 3. First results ###

> You can now view the monitoring: deploy the war and open the following page in a web navigator after starting the server:

> `http://<host>/<context>/monitoring`

> where `<host>` is the name of the server where the webapp is deployed, followed possibly by the port (for example localhost:8080)
> and where `<context>`  is the name of the context of the webapp which you have configured during the deployment thereof.

> If the starting of the server does not work and if you have an error in the output of the server complaining about a "window server" (in particular if you use Mac OS X server), add the system property "-Djava.awt.headless=true" (in the java launch command or in the administration user interface of the server; or if you use tomcat, you can add "java.awt.headless=true" in the file $CATALINA\_HOME/conf/catalina.properties). And restart the server.

> Then you can complete the settings as below according to your needs. And before using it in production, you probably want to secure the monitoring page using your own means or see the [security](#16._Security.md) chapter.

### 4. PDF report generation ###

> To generate PDF reports as well as html reports, adding the library iText [v2.1.x](http://search.maven.org/remotecontent?filepath=com/lowagie/itext/2.1.7/itext-2.1.7.pom) is required
> (licence [LGPL or MPL](http://search.maven.org/remotecontent?filepath=com/lowagie/itext/2.1.7/itext-2.1.7.pom), the iText jar file alone is enough, without the other dependencies like iText-rtf).

> Copy the file `itext-2.1.7.jar`, located in the directory `src/test/test-webapp/WEB-INF/lib/` of the supplied javamelody.zip file,
> to the WEB-INF/lib directory of the war of the webapp to monitor. The PDF link will be available at the top of the html report.

### 5. Supplements in `web.xml` ###

> `url-pattern` in `web.xml` file above can be adapted in order not to monitor some urls, possibly with several mappings on the same filter.

> For example, use the following `filter-mapping`s and `url-pattern`s to monitor only urls like `/servlet1/*` and `/servlet2/*` but not the others like `/static/*` :

```
	<filter>
		<filter-name>javamelody</filter-name>
		<filter-class>net.bull.javamelody.MonitoringFilter</filter-class>
	</filter>
	<filter-mapping>
		<filter-name>javamelody</filter-name>
		<url-pattern>/servlet1/*</url-pattern>
	</filter-mapping>
	<filter-mapping>
		<filter-name>javamelody</filter-name>
		<url-pattern>/servlet2/*</url-pattern>
	</filter-mapping>
	<filter-mapping>
		<filter-name>javamelody</filter-name>
		<url-pattern>/monitoring</url-pattern>
	</filter-mapping>
	<listener>
		<listener-class>net.bull.javamelody.SessionListener</listener-class>
	</listener>
```

> Note that the `url-pattern`s must "contain" the url `/monitoring` in order to have reports.

> As an alternative, you can use a single `/*` `url-pattern` and a filter parameter with a [regular expression](http://java.sun.com/javase/6/docs/api/java/util/regex/Pattern.html) can be added before ` </filter> ` in order to exclude some urls, for example:

```
	<filter>
		<filter-name>javamelody</filter-name>
		<filter-class>net.bull.javamelody.MonitoringFilter</filter-class>
		
		<init-param>
			<param-name>url-exclude-pattern</param-name>
			<param-value>/static/.*</param-value>
		</init-param>
	</filter>
	<filter-mapping>
		<filter-name>javamelody</filter-name>
		<url-pattern>/*</url-pattern>
	</filter-mapping>
	<listener>
		<listener-class>net.bull.javamelody.SessionListener</listener-class>
	</listener>
```

### 6. Optional parameters ###

> It is possible to configure some settings. These parameters can be defined in ascending order of priority:

  * in initialisation parameters of the filter
> > (`web.xml` file in the webapp), for example:

```
	<filter>
		<filter-name>javamelody</filter-name>
		<filter-class>net.bull.javamelody.MonitoringFilter</filter-class>
		<init-param>
			<param-name>log</param-name>
			<param-value>true</param-value>
		</init-param>
	</filter>
	...
```

  * in context parameters of the webapp with prefix `javamelody.`


> In the `web.xml` file in the webapp, for example:

```
	 <context-param>
	 	<param-name>javamelody.log</param-name>
	 	<param-value>true</param-value>
	 </context-param>
	...
```

> Or in the context xml file of Tomcat conf or in Glassfish, for example:

```
	<?xml version="1.0" encoding="UTF-8" ?>
	<Context docBase="path_to/mywebapp.war" path="mywebapp" reloadable="false" >
		<Parameter name='javamelody.log' value='true' override='false'/>
	</Context>
```

  * in system properties with prefix `javamelody.`
> > (java launch command or administration user interface of the server or $CATALINA\_HOME/conf/catalina.properties if tomcat), for example:

```
	... -Djavamelody.log=true
```


> The parameter `system-actions-enabled` (`true` by default) enables or disables the system actions garbage collector, http sessions, heap dump, memory histogram, process list, jndi tree, opened jdbc connections, database (near the bottom of reports).
> These actions have confirmations when necessary.

> The parameter `url-exclude-pattern` is a
> [regular expression](http://java.sun.com/javase/6/docs/api/java/util/regex/Pattern.html)
> to exclude some urls from monitoring as written above.

> The parameter `http-transform-pattern` is a
> [regular expression](http://java.sun.com/javase/6/docs/api/java/util/regex/Pattern.html)
> to transform descriptions of http requests and to delete dynamic parts (identifiers of objects for example)
> in order to be able to aggregate on these requests

> Similarly, the parameter `sql-transform-pattern` is a
> [regular expression](http://java.sun.com/javase/6/docs/api/java/util/regex/Pattern.html)
> to transform descriptions of sql requests (not binded identifiers for a "in" clause for example)
> in order to be able to aggregate on these requests.

> And the parameters `ejb-transform-pattern`, `spring-transform-pattern`, `guice-transform-pattern`, `error-transform-pattern`, `log-transform-pattern`, `job-transform-pattern`, `jsf-transform-pattern`, `struts-transform-pattern` and `jsp-transform-pattern` can transform descriptions of
> ejb3 methods, of spring methods, of guice methods, of http system errors, of system error logs, of names of jobs, of jsf actions, of struts actions or of jsp pages (in order to aggregate by ejb components and not by ejb methods for example)

> The parameter `displayed-counters` can modify the displayed counters for the statistics and for the graphics (`http,sql,error,log` by default). So the counters displayed by default are those with keys http, sql, error and log. The counters with keys jsf, struts, jsp, ejb, spring, guice and services are automatically displayed when they are used (see below for ejb, spring, guice or struts and see the UserGuideAdvanced for services without ejb, spring and guice).
> So the parameter `displayed-counters` can hide some counters with for example the value `http,sql,ejb,spring`.

> The parameter `log` can activate logs of http requests at the INFO level (`false` by default).
> The http request will be logged with the duration and the size of the response
> in the log category corresponding to the name of the filter set in `web.xml` file,
> using Logback or Log4J if present in your application, or using java.util.logging otherwise.

> The parameter `storage-directory` is the name of the directory of storage (javamelody by default).
> If the name of the directory starts with '/' (or on Windows, with drive specifier followed by '\', or if its prefix is "\\"), it is considered as an absolute path,
> otherwise it is considered as relative to the temporary directory
> (`<temp>`  in `TOMCAT_HOME` for tomcat).
> If this parameter is changed it is recommended to rename the physical directory at the same time.

> The parameter `resolution-seconds` is the resolution of charts in seconds (60 by default).
> A resolution between 60 and 600 is recommended (ie 1 to 10 minutes).
> If this parameter is decreased, stored `*.rrd` files should be deleted for the parameter to be taken into account.

> The parameters `warning-threshold-millis` and `severe-threshold-millis` are the thresholds in milliseconds
> (global mean + 1 standard deviation and global mean + 2 x standard deviation by default,
> this default setting gives dynamic thresholds which indicate the requests with unusual mean times regardless of the
> application).
> Beyond the thresholds the mean times are displayed in orange or in red and are separately counted in summary tables
> with their average time percentages, hits, etc.
> These threshold parameters can serve as a basis for a SLA (service level) of an application, for which
> constraints can be defined such as "response time less than 2s for 90% of http requests".

> The parameter `monitoring-path` (`/monitoring` by default) is used to change the path in the URL to open
> the report of the monitoring. For example, `http://.../admin/performance` in place of `http://.../monitoring`

> The parameter `gzip-compression-disabled` disables the compression of the monitoring reports,
> for example if there is another mechanism which would compress a second time
> (`false` by default, the reports are compressed by default if the browser supports it).

> The parameter `no-database` just disables the monitoring of jdbc connections, the monitoring of sql requests and the reports on the database in system information.
> Set it to true to disable all that.

> The parameter `disabled` (`false` by default) just disables the monitoring.
> This allows for example to disable the monitoring temporarily or only on some servers,
> from the tomcat context or from system properties without modifying the `web.xml` file
> neither the war file of the monitored webapp.

### 7. JDBC ###

> If a DataSource, which JNDI name is like "jdbc/MyDataSource", is configured in the application server
> (xml context of the webapp in Tomcat for example), the sql requests will be automatically monitored
> without requiring any parameters (tested on Tomcat 5.5, 6 and 7, glassfish 3, jboss 5, weblogic 11g, jetty 6). Note that if JndiObjectFactoryBean is used for the DataSource in Spring, then SessionListener should be before ContextLoaderListener in the web.xml file or monitoring-spring.xml should be used as said below.

> If a jdbc driver is used directly without DataSource, "net.bull.javamelody.JdbcDriver" should be defined
> as class of driver and the jdbc property "driver" should be added with the class of the real driver for value.
> For example, if you use a hibernate.cfg.xml file and mysql (without hibernate.connection.datasource):

```
	<property name="hibernate.connection.driver_class">net.bull.javamelody.JdbcDriver</property>
	<property name="hibernate.connection.driver">com.mysql.jdbc.Driver</property>
	<property name="hibernate.connection.url">jdbc:mysql://localhost:3306/myschema</property>
	<property name="hibernate.connection.username">myuser</property>
	<property name="hibernate.connection.password">mypassword</property>
```

> If a DataSource is used but its JNDI name is not like "jdbc/MyDataSource" or if this DataSource
> is not in one of the usual JNDI contexts "java:comp/env/" or "java:/", then you can add the optional parameter
> `datasources` (with a system property, a parameter of the context or of the filter) to define the JNDI name
> of the datasource used by the application. If there are several datasources, this parameter can contain
> the JNDI name of the datasources separated by commas. If a jonas v5 server is used, datasources can be monitored
> but it appears that the parameter `datasources` must be used to declare them.

> For example, for a system property in server launch command:

> `-Djavamelody.datasources=java:comp/env/myapp/MyDataSource`

> If a DataSource is defined in a xml file of a Spring context,
> like for example ` <bean class="org.apache.commons.dbcp.BasicDataSource">...</bean> ` or ` <bean class="org.springframework.jndi.JndiObjectFactoryBean">...</bean> `,
> the DataSource can also be monitored, with a Spring post-processor.
> Make sure the Spring configuration file (`net/bull/javamelody/monitoring-spring.xml`,
> included in the provided jar) is loaded as one of the first configuration files.
> For example, if you use the `org.springframework.web.context.ContextLoaderListener` in your
> `web.xml`, the `contextConfigLocation` context parameter will look something like this:

```
	<context-param>
	  <param-name>contextConfigLocation</param-name>
	  <param-value>
		classpath:net/bull/javamelody/monitoring-spring.xml
		classpath:context/services.xml
		classpath:context/data-access-layer.xml
		/WEB-INF/applicationContext.xml
	  </param-value>
	</context-param>
```

> If ever, there is a conflict in your application between the all-in-one monitoring-spring.xml and AOP or @Autowired,
> then you can use the monitoring-spring-datasource.xml file, instead of monitoring-spring.xml.
> This file contains only the datasource post-processor and an example of SpringDataSourceFactoryBean.

### 8. Business facades (ejb-jar.xml file if EJB3) ###

> If the application to monitor contains some business façades in EJB3 (Java EE 5) with annotations `@Stateless`,
> `@Stateful` or `@MessageDriven`, a counter can be created for statistics of execution of methods.
> But it is not recommended for response time to monitor execution of all the methods of EJB3 beans,
> for example the methods of low granularity as getters of entities.

> To configure this, declare in your `ejb-jar.xml` file the ejb3 interceptor as in the following example:

```
	<ejb-jar
		xmlns = "http://java.sun.com/xml/ns/javaee"
		version = "3.0"
		xmlns:xsi = "http://www.w3.org/2001/XMLSchema-instance"
		xsi:schemaLocation = "http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/ejb-jar_3_0.xsd"
	>
		<interceptors>
			<interceptor>
				<interceptor-class>net.bull.javamelody.MonitoringInterceptor</interceptor-class>
			</interceptor>
		</interceptors>
		<assembly-descriptor>
			<interceptor-binding>
				<ejb-name>*</ejb-name>
				<interceptor-class>net.bull.javamelody.MonitoringInterceptor</interceptor-class>
			</interceptor-binding>
		</assembly-descriptor>
	</ejb-jar>
```

or

```
	<ejb-jar
		xmlns = "http://java.sun.com/xml/ns/javaee"
		version = "3.0"
		xmlns:xsi = "http://www.w3.org/2001/XMLSchema-instance"
		xsi:schemaLocation = "http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/ejb-jar_3_0.xsd"
	>
		<interceptors>
			<interceptor>
				<interceptor-class>net.bull.javamelody.MonitoringInterceptor</interceptor-class>
			</interceptor>
		</interceptors>
		<assembly-descriptor>
			<interceptor-binding>
				<ejb-name>FacadeBean1</ejb-name>
				<interceptor-class>net.bull.javamelody.MonitoringInterceptor</interceptor-class>
			</interceptor-binding>
			<interceptor-binding>
				<ejb-name>FacadeBean2</ejb-name>
				<interceptor-class>net.bull.javamelody.MonitoringInterceptor</interceptor-class>
			</interceptor-binding>
		</assembly-descriptor>
	</ejb-jar>
```

> In this file, it is possible to use `*` for all ejb or to list names of ejb which should be monitored.
> If you have several ejb-jar.xml files, configure the interceptor in each of them accordingly.
> Or without modifying the ejb-jar.xml file, it is possible to add the annotation @Interceptor in java sources
> of ejb implementations.

### 9. Business facades (if Spring) ###

> If the application to monitor contains some business façades or other objects initialized with Spring,
> a counter can be created for statistics of execution of methods. But it is not recommended for response time
> to monitor execution of all methods of Spring beans, for example the methods of low granularity as getters of entities.

> Step 1: Add the jar dependency spring-aop to your project (in pom.xml if maven).

> Step 2: Make sure the Spring configuration file (`net/bull/javamelody/monitoring-spring.xml`,
> [included](http://code.google.com/p/javamelody/source/browse/trunk/javamelody-core/src/main/resources/net/bull/javamelody/monitoring-spring.xml) in the provided jar) is loaded as one of the first configuration files.

> For example, if you use the `org.springframework.web.context.ContextLoaderListener` in your
> `web.xml` the `contextConfigLocation` context parameter will look something like this:

```
	<context-param>
	  <param-name>contextConfigLocation</param-name>
	  <param-value>
		classpath:net/bull/javamelody/monitoring-spring.xml
		classpath:context/services.xml
		classpath:context/data-access-layer.xml
		/WEB-INF/applicationContext.xml
	  </param-value>
	</context-param>
```

> Step 3: Assuming that the business façades have a common interface (or super-class)
> `com.xyz.someapp.service.Facade` and that you want to monitor all methods of those façades,
> add the following lines in your applicationContext.xml file:

```
	<bean id="facadeMonitoringAdvisor" class="net.bull.javamelody.MonitoringSpringAdvisor">
		<property name="pointcut">
			<bean class="net.bull.javamelody.MonitoredWithInterfacePointcut">
				<property name="interfaceName" value="com.xyz.someapp.service.Facade" />
			</bean>
		</property>
	</bean>
```

> As the Spring configuration of the monitoring is only for spring beans, make sure these classes are
> instantiated through Spring (i.e. declare them as a bean in a Spring configuration file).

> Note: If you don't have a common interface (or super-class) for façades or if you don't want to monitor all façades,
> you can create and add to your façades an interface `com.xyz.someapp.service.Monitored`
> and then use it in your xml file as above, otherwise you can use an alternative as below.

> If you prefer, you have the choice with other alternatives for step 3.
> These alternatives are not exclusive: you can use one and complete it with another.

  * Alternative to step 3: Use of an annotation of JavaMelody.
> (implies that the library javamelody.jar is included in your classpath for compilation)

> Just add the annotation `@net.bull.javamelody.MonitoredWithSpring` to all implementation classes and/or interfaces and/or methods of interfaces which you want to monitor, without modification of your applicationContext.xml file. For example:

```
	@net.bull.javamelody.MonitoredWithSpring
	public interface Test {
		void myMethod();
	}
	
	or
	
	public interface Test {
		@net.bull.javamelody.MonitoredWithSpring
		void myMethod();
	}
	
	or
	
	@net.bull.javamelody.MonitoredWithSpring
	public class TestImpl implements Test {
		public void myMethod() {
			...
		}
	}
```

> This alternative with annotation also lets you change the names of displayed classes in the monitoring.
> If the name displayed by the monitoring for the class or the method does not please you,
> you can add (name="my business use case") in brackets after the annotation on your class or on your method.

  * Alternative to step 3: Use of JdkRegexpMethodPointcut.
> Assuming that the business façades can be found with a regular expression,
> add the following lines in your applicationContext.xml file and adapt the value of the regular expression pattern:

```
	<bean id="facadeMonitoringAdvisor" class="net.bull.javamelody.MonitoringSpringAdvisor">
		<property name="pointcut">
			<bean class="org.springframework.aop.support.JdkRegexpMethodPointcut">
				<property name="pattern" value=".*Facade.*" />
			</bean>
		</property>
	</bean>
```

  * Alternative to step 3: Use of any pointcut provided by Spring or writing of your own pointcut.
> You can use any [pointcut](http://static.springsource.org/spring/docs/2.5.x/api/index.html)
> provided by Spring or you can write your own.
> For example, you can create a class `com.xyz.someapp.service.MonitoringPointcut`
> which implements org.springframework.aop.Pointcut and add the following lines in your applicationContext.xml file:

```
	<bean id="facadeMonitoringAdvisor" class="net.bull.javamelody.MonitoringSpringAdvisor">
		<property name="pointcut">
			<bean class="com.xyz.someapp.service.MonitoringPointcut" />
		</property>
	</bean>
```

> Note : The [documentation](http://static.springsource.org/spring/docs/2.5.6/reference/aop.html#aop-proxying) and [blog](http://blog.springsource.com/2007/07/19/debunking-myths-proxies-impact-performance/)
> of Spring AOP advises to use JDK dynamic proxies and not CGLIB, which should pose no problem for facades with separate interfaces and implementations.

  * Alternative in order to use AspectJ, in case that proxies don't work (thanks to [Witek Wolejszo](http://touk.pl/blog/2011/03/07/javamelody-spring-and-aspectj/))
> Sometimes proxies can't work is some circumstances, for example if you don't use interfaces.
> In this case, it is possible to use AspectJ with the following context file:
> `net/bull/javamelody/monitoring-spring-aspectj.xml`, [included](http://code.google.com/p/javamelody/source/browse/trunk/javamelody-core/src/main/resources/net/bull/javamelody/monitoring-spring-aspectj.xml) in the provided jar, instead of
> `net/bull/javamelody/monitoring-spring.xml`.

> You can then use the @MonitoredWithSpring annotation as said above,
> or you can use other pointcuts like the following with a common interface (or super-class):

```
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
            http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-3.0.xsd
            http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-3.0.xsd">

    <aop:config proxy-target-class="true" >
        <aop:advisor advice-ref="monitoringAdvice" pointcut-ref="facadePointcut" />
    </aop:config>

	<bean id="facadePointcut" class="net.bull.javamelody.MonitoredWithInterfacePointcut">
		<property name="interfaceName" value="com.xyz.someapp.service.Facade" />
	</bean>
</beans>
```

### 10. Business facades (if Guice) ###

> If the application to monitor contains some business facades or other objects initialized with Guice,
> a counter can be created for statistics of execution of methods. But it is not recommended for response time
> to monitor execution of all methods of Guice beans, for example the methods of low granularity as getters of entities.

> Step 1: Add the jar dependency guice to your project (in pom.xml if maven).
> As pre-requisite for guice, the jar of javamelody must be available in the classpath used to compile sources.

> Step 2 (with annotations): Add the MonitoringGuiceModule to your Guice module. For example:

```
		final Module testModule = new AbstractModule() {
			/** {@inheritDoc} */
			@Override
			protected void configure() {
				// configuration of the Guice monitoring with annotations
				install(new net.bull.javamelody.MonitoringGuiceModule());
				// facade
				bind(TestFacade.class).to(TestFacadeImpl.class);
			}
		};
```

> Then, just add the annotation `@net.bull.javamelody.MonitoredWithGuice` to all
> implementation classes (not to interfaces) and/or methods you want to monitor. For example:

```
	@net.bull.javamelody.MonitoredWithGuice
	public class TestImpl implements Test {
		public void myMethod() {
			...
		}
	}
	
	or
	
	public class TestImpl implements Test {
		@net.bull.javamelody.MonitoredWithGuice
		public void myMethod() {
			...
		}
	}
```

> The annotation also lets you change the names of displayed classes in the monitoring.
> If the name displayed by the monitoring for the class or the method does not please you,
> you can add (name="my business use case") in brackets after the annotation on your class
> or on your method.

> As the Guice configuration of the monitoring is only for guice beans, make sure these classes are
> instantiated through Guice (i.e. declare them as a bind in a Guice module).

> As alternative to step 2, you can use an interface or a super-class in place of annotations.
> Assuming that the business facades have a common interface or super-class
> `com.xyz.someapp.service.Facade` and that you want to monitor all methods of those facades,
> add only the following bindInterceptor in your Guice module:

```
		final Module testModule = new AbstractModule() {
			/** {@inheritDoc} */
			@Override
			protected void configure() {
				// configuration of the Guice monitoring with interface
				bindInterceptor(Matchers.subclassesOf(com.xyz.someapp.service.Facade.class),
					Matchers.any(), new net.bull.javamelody.MonitoringGuiceInterceptor());
				// facade
				bind(TestFacade.class).to(TestFacadeImpl.class);
			}
		};
```

> Note: If you don't have a common interface for facades or if you don't want to monitor all facades,
> you can create and add to your facades an interface `com.xyz.someapp.service.Monitored`
> and then use it in your module as above.

### 11. JSF Actions ###

> If the application to monitor contains some JSF actions based on Mojarra (JSF Reference Implementation),
> a counter is automatically created for statistics of execution of actions.
> A JSF ActionListener is automatically added for the JSF application.
> You don't need to do anything to have JSF statistics.

### 12. Struts 2 Actions ###

> If the application to monitor contains some [Struts 2](http://struts.apache.org/) actions,
> a counter can be created for statistics of execution of actions.
> For this, a Struts interceptor must be added.

> In your Struts xml file, add the following lines (or add the "monitoring" interceptor
> in your default stack) :

```
	<package name="default" extends="struts-default" >
		<interceptors>
			<interceptor name="monitoring" class="net.bull.javamelody.StrutsInterceptor"/>          

			<interceptor-stack name="myStack">
				<interceptor-ref name="monitoring"/>
				<interceptor-ref name="defaultStack"/>
			</interceptor-stack>
		</interceptors>
		
		<default-interceptor-ref name="myStack"/>
	</package>
```

> Then, in all your Struts packages, use the "default" package with `extends="default"`

### 13. Batch jobs (if Quartz) ###

> Batch jobs are automatically monitored when they are scheduled with [Quartz](http://www.quartz-scheduler.org/).

> But if these jobs are scheduled in Quartz using
> [Spring scheduling](http://static.springsource.org/spring/docs/2.5.x/reference/scheduling.html),
> you need to add the javamelody parameter "quartz-default-listener-disabled" with the value "true"
> and you need to add the property "exposeSchedulerInRepository" with the value "true" for the bean
> "SchedulerFactoryBean" in your applicationContext.xml file.

```
	<bean id="quartzScheduler" class="org.springframework.scheduling.quartz.SchedulerFactoryBean">
		<property name="exposeSchedulerInRepository" value="true" />
		...
	</bean>
```

> Without this "exposeSchedulerInRepository" property, the jobs scheduled using Spring are neither
> monitored nor displayed in report.

### 14. Weekly, daily or monthly reports by mail ###

> A weekly, daily or monthly report in pdf format can be sent by mail to one or several people.
> This functionality needs the library iText in your webapp (licence LGPL or MPL, the iText jar file
> alone is enough, without the other dependencies like iText-rtf). And this needs to have JavaMail and Activation
> from Sun in the libraries of the server for the mail session.

> For this, if the webapp is in tomcat, download the jar files
> http://repo1.maven.org/maven2/javax/mail/mail/1.4.1/mail-1.4.1.jar
> and http://repo1.maven.org/maven2/javax/activation/activation/1.1/activation-1.1.jar.
> Then if it is tomcat 5.x, move the 2 jar files in `<tomcat_home>/common/lib`
> and if it is tomcat 6.x, move the 2 jar files in `<tomcat_home>/lib`

> And add a mail session in the configuration of the application server with the name of your smtp server.
> For example, add the following in the tomcat context of your webapp (xml file located in
> `<tomcat_home>/conf/Catalina/localhost/` or in `<tomcat_home>/conf/server.xml`):

```
	<Resource name="mail/MySession" auth="Container" type="javax.mail.Session"
		mail.smtp.host="<smtp server>"
		mail.smtp.user="<login>"
		mail.from="no-reply@example.com"
	/>
```

> where the name of the session is of your choice and where the properties depend on your mail server.
> If the mail server needs an authentication, it is possible to add in the properties:

```
	mail.smtp.auth=true
	mail.smtp.password=<password>
```

> If the mail server needs ssl, replace mail.smtp by mail.smtps on all lines above and add a line

> `mail.transport.protocol=smtps`

> Other configuration parameters can be added if necessary for the
> [mail](https://javamail.java.net/nonav/docs/api/com/sun/mail/smtp/package-summary.html) session

> Then it is necessary to add the list of mail addresses separated by commas to the parameter
> `admin-emails`, in `web.xml` file or in tomcat context or in system properties like the optional parameters
> above.

> And the name of the mail session should also be added in the parameter `mail-session`.
> For example, for the tomcat context of your webapp:

```
	<Parameter name='javamelody.admin-emails' value='admin1@societe.fr,admin2@societe.fr' override='false'/>
	<Parameter name='javamelody.mail-session' value='mail/MySession' override='false'/>
```

> By default, the report is sent weekly (once a week).
> It is possible to adapt that to the desired level of supervision.
> For example, it is possible to send it daily (every day),
> monthly, or a combination of daily, weekly and monthly.

> For that, add the parameter `mail-periods` with the periods "day", "week" or "month" separated by commas.
> For example, for the tomcat context of your webapp with a daily period:

```
<Parameter name='javamelody.mail-periods' value='day' override='false'/>
```

> Or for daily, weekly and monthly periods:

```
<Parameter name='javamelody.mail-periods' value='day,week,month' override='false'/>
```

> Finally, the tomcat context of your webapp could look like this one:

```
<?xml version="1.0" encoding="UTF-8" ?> 
<Context docBase="/path_to_mywebapp/mywebapp.war" path="mywebapp" reloadable="false" > 
	<Resource name="mail/MySession" auth="Container" type="javax.mail.Session"
		mail.smtp.host="<serveur smtp>"
		mail.smtp.user="<login>"
		mail.from="no-reply@example.com"
	/>
	<Parameter name='javamelody.admin-emails' value='admin1@societe.fr,admin2@societe.fr' override='false'/>
	<Parameter name='javamelody.mail-session' value='mail/MySession' override='false'/>
	<Parameter name='javamelody.mail-periods' value='day,week,month' override='false'/>
</Context>
```

> Once the server is started, you can send a test mail by calling this action: `http://<host>/<context>/monitoring?action=mail_test`

### 15. Database user ###

> From the html page of the monitoring, it is possible to display information and statistics
> on the database (if postgresql, mysql or oracle),
> like for example the current sql requests or if oracle, the longest requests in cumulative time with
> display of the cpu time and of the elementary cost (of buffer gets).

> For that, the user in the database used by the monitored application must have the necessary
> rights to read those information and statistics. For example the right to execute the request
> `select * from pg_stat_activity` in postgresql, or the request `show full processlist`
> in mysql or the request `select * from v$session` in oracle.

> If the user in the database does not already have the necessary rights, you should grant them.
> For example, in oracle you can execute as a 'system' user the following sql request
> in order to grant the right to the user 'myapplication':
> `grant select any dictionary to myapplication`

### 16. Security ###

> The monitoring page does not contain data such as passwords, but before using it in production, you may probably want that this page is in restricted access. If you use a Jenkins, JIRA, Confluence, Bamboo or Liferay plugin, it is secured by the app roles. Or, you may secure it by using your own means.

> Otherwise, it is possible to restrict its access by a regular expression on ip address of client with the parameter `allowed-addr-pattern`
> (regular expression with a range of internal ip addresses or fixed ip addresses of administrators).
> For example, "`192\.168\..*|123\.123\.123\.123`" to allow the ip addresses in the "`192.168.*.*`" range plus any pc hiding behind the gateway `123.123.123.123`.

> Note that if you use a http proxy server like Apache in front of the server, the ip address of the client will be the one of Apache. So you may not use `allowed-addr-pattern` in this case, or do not use Apache to access this page, or you may enable mod\_proxy\_ajp in order that the monitored server knows the ip address of the clients. Example of httpd conf with ajp:

```
	<location /webapp>
		ProxyPass ajp://localhost:8080/webapp
	</location> 
```

> Or if some `security-constraint` et `security-role`(s) are defined in the `web.xml` file of the application,
> you can also restrict in the `web.xml` file the access to the monitoring page to "monitoring" role
> for example.

> Example of content of the web.xml file for authentication by login and password:

```
	<login-config>
		<auth-method>BASIC</auth-method>
		<realm-name>Monitoring</realm-name>
	</login-config>
	<security-role>
		<role-name>monitoring</role-name>
	</security-role>
	<security-constraint>
		<web-resource-collection>
			<web-resource-name>Monitoring</web-resource-name>
			<url-pattern>/monitoring</url-pattern>
		</web-resource-collection>
		<auth-constraint>
			<role-name>monitoring</role-name>
		</auth-constraint>
                <!-- if SSL enabled (SSL and certificate must then be configured in the server)
                <user-data-constraint>
                        <transport-guarantee>CONFIDENTIAL</transport-guarantee>
                </user-data-constraint> 
                -->
	</security-constraint>
```

> The realm and the users must be defined in the application server,
> and the users must have the "monitoring" role to have access to the reports.
> For example, if tomcat is used with the default realm, modify the content of the conf/tomcat-users.xml file as follows:

```
	<?xml version='1.0' encoding='utf-8'?>
	<tomcat-users>
	     <role rolename="monitoring"/>
	     <user username="monitoring" password="monitoring" roles="monitoring"/>
	</tomcat-users>
```

> Or, if you want BASIC authentication with username and password, but do no want to use a realm and "security-constraint" in web.xml, you can add the parameter "authorized-users" in web.xml, in context or in system properties like the other [javamelody parameters](#6._Optional_parameters.md) (since v1.53). For example in your WEB-INF/web.xml file:

```
	<filter>
		<filter-name>javamelody</filter-name>
		<filter-class>net.bull.javamelody.MonitoringFilter</filter-class>
		<init-param>
			<param-name>authorized-users</param-name>
			<param-value>user1:pwd1, user2:pwd2</param-value>
		</init-param>
	</filter>
	...
```

### 17. SecurityManager ###

> If your are one of the rare people with a security manager enabled in your server, there will be
> an exception like the following one in the server output at the start of the webapp with JavaMelody:
> "java.security.AccessControlException: access denied ...".

> It is because JavaMelody needs access to a lot of system data and it needs to write its files.
> In order for JavaMelody to work, you can disable the SecurityManager (if Tomcat, remove the option
> [-security](http://tomcat.apache.org/tomcat-6.0-doc/security-manager-howto.html) from start)
> or if you want to keep it, you can modify the
> [java.policy](http://java.sun.com/javase/6/docs/technotes/guides/security/PolicyFiles.html#Examples)
> file of your server.

> For example with Tomcat, the java.policy file is $CATALINA\_HOME/conf/catalina.policy
> and you can add something like the following example to grant access to the JavaMelody and JRobin jar files:

```
	grant codeBase "file:${catalina.home}/webapps/<your_webapp>/WEB-INF/lib/javamelody-x.jar" {
		permission java.security.AllPermission;
	};
	grant codeBase "file:${catalina.home}/webapps/<your_webapp>/WEB-INF/lib/jrobin-1.5.9.1.jar" {
		permission java.security.AllPermission;
	};
```

> and replace `<your_webapp>` with the name of your webapp and replace
> `javamelody-x.jar` with the exact name of the JavaMelody jar file.

### 18. Full results ###

> To view the monitoring: deploy the war and open the following page in a web navigator after starting the server:

> `http://<host>/<context>/monitoring`

> where `<host>` is the name of the server where the webapp is deployed, followed possibly by the port (for example localhost:8080)
> and where `<context>` is the name of the context of the webapp which you have configured during the deployment thereof.


## Dependencies ##

> If you don't have Maven, the jar files can be found in the zip file in "[Download](https://github.com/javamelody/javamelody/releases)" inside the root directory and inside the directory `src/test/test-webapp/WEB-INF/lib`

> If you have Maven, you can use the dependencies `javamelody-core` (`jrobin` is included transitively with javamelody-core), and optionally `itext` to add PDF export, in your `pom.xml` like this:

```
	<!-- javamelody-core -->
	<dependency>
		<groupId>net.bull.javamelody</groupId>
		<artifactId>javamelody-core</artifactId>
		<version>1.56.0</version>
	</dependency>
	<!-- itext, option to add PDF export -->
	<dependency>
		<groupId>com.lowagie</groupId>
		<artifactId>itext</artifactId>
		<version>2.1.7</version>
		<exclusions>
			<exclusion>
				<artifactId>bcmail-jdk14</artifactId>
				<groupId>bouncycastle</groupId>
			</exclusion>
			<exclusion>
				<artifactId>bcprov-jdk14</artifactId>
				<groupId>bouncycastle</groupId>
			</exclusion>
			<exclusion>
				<artifactId>bctsp-jdk14</artifactId>
				<groupId>bouncycastle</groupId>
			</exclusion>
		</exclusions>
	</dependency>
```

The dependencies `jrobin` (included in javamelody-core) and if you wish `xstream` as an option for the xml and json export or for the remoting with the collector server are:

```
	<dependency>
		<groupId>com.thoughtworks.xstream</groupId>
		<artifactId>xstream</artifactId>
		<version>1.4.2</version>
	</dependency>
	<dependency>
		<groupId>org.jrobin</groupId>
		<artifactId>jrobin</artifactId>
		<version>1.5.9</version>
	</dependency>
```

## Advanced documentation (setup with an ear, centralized collect server, sql requests in GlassFish v3, JBoss AS 7, JonAS 5, debugging logs...) ##

> To setup with an ear, to deploy a centralized collect server, to monitor sql requests in GlassFish v3, to use JBoss AS 7, JonAS 5 or to enable debugging logs, see [UserGuideAdvanced](UserGuideAdvanced.md)

## Compilation and development ##

> See [DevGuide](DevGuide.md)