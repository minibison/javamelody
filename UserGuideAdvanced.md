# Advanced documentation of Java Melody #

For standard documentation, see [UserGuide](UserGuide.md)

## Table of contents ##

  * [Setup JavaMelody in an ear file](#Setup_in_an_ear_file.md)
    * [1. Jar files](#1._Jar_files.md)
    * [2. web.xml file](#2._web.xml_file.md)
    * [3. First results](#3._First_results.md)
    * [4 to 17. Other settings (optional parameters, JDBC, EJB3, etc)](#4_to_17._Other_settings_(optional_parameters,_JDBC,_EJB3,_etc).md)
  * [Optional centralization server setup](#Optional_centralization_server_setup.md)
    * [1. War file of the webapp of monitoring](#1._War_file_of_the_webapp_of_monitoring.md)
    * [2. Deployment of the webapp of monitoring](#2._Deployment_of_the_webapp_of_monitoring.md)
    * [3. Simpler alternative of deployment of the webapp of monitoring](#3._Simpler_alternative_of_deployment_of_the_webapp_of_monitoring.md)
    * [4. Results with a collect server](#4._Results_with_a_collect_server.md)
    * [5. Security with a collect server](#5._Security_with_a_collect_server.md)
    * [6. If reverse proxy](#6._If_reverse_proxy.md)
  * [Business facades (without EJB3 and without Spring and without Guice)](#Business_facades_(without_EJB3_and_without_Spring_and_without_Gu.md)
  * [Enable Hotspots detection](#Enable_Hotspots_detection.md)
  * [JPA monitoring](#JPA_monitoring.md)
  * [Alternative for monitoring of sql requests](#Alternative_for_monitoring_of_sql_requests.md)
  * [Monitoring of sql requests and of jdbc connections in GlassFish v3+](#Monitoring_of_sql_requests_and_of_jdbc_connections_in_v3+.md)
  * [Usage of JavaMelody in JonAS 5 (which uses OSGI)](#Usage_of_in_JonAS_5_(which_uses_OSGI).md)
  * [Deployment on Tomcat without modification of monitored webapps (beta)](#Deployment_on_Tomcat_without_modification_of_monitored_webapps_(.md)
  * [Embedding JavaMelody in a standalone app](#Embedding_in_a_standalone_app.md)
  * [Debugging logs](#Debugging_logs.md)
  * [Clearing all statistics and all graphs](#Clearing_all_statistics_and_all_graphs.md)
  * [Custom reports](#Custom_reports.md)
  * [Customizing styles, icons and other resources in the html reports](#Customizing_styles,_icons_and_other_resources_in_the_html_report.md)
  * [Using a servlet to display the monitoring reports](#Using_a_servlet_to_display_the_monitoring_reports.md)
  * [Report written before last shutdown](#Report_written_before_last_shutdown.md)
  * [Format of RRD files](#Format_of_RRD_files.md)
  * [Compilation and development](#Compilation_and_development.md)


## Setup JavaMelody in an ear file ##

> (Written based on a contribution by 'dhartford')

> If you deploy your application with a war file or with an equivalent directory, just follow the [UserGuide](UserGuide.md).

> If you deploy your application with an ear file, probably because you use EJBs, this chapter will guide you to setup JavaMelody in your ear.

### 1. Jar files ###

> Copy the files `javamelody.jar` and `jrobin-x.jar`, located at the root of the supplied javamelody.zip file,
> to the lib directory of the ear of the application to monitor (lib directory is "required" by the JavaEE 5 specification).
> If you want to have reports in PDF format or weekly reports by mail, copy also the `itext-x.jar` file to the same location.

> To declare the jar files in the ear, modify the `/META-INF/application.xml` file in your ear like this:

```
	<application version="5"
		xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/application_5.xsd">
		....
		<module>
			<java>lib/javamelody.jar</java>
		</module>
		<module>
			<java>lib/jrobin-x.jar</java>
		</module>
		<module>
			<java>lib/itext-x.jar</java>
		</module>
		<module>
			<web>
				<web-uri>mywebapp.war</web-uri>
				<context-root>mycontext</context-root>
			</web>
		</module>
		....
	</application>
```

> If you use Maven, you may want to use the maven ear plugin and the `includeInApplicationXml` flag to make this file.

### 2. `web.xml` file ###

> You need to have the war of a single monitored webapp in your ear.
> If you do not have a war, you need to create and deploy a war just for having the monitoring: create it with a simple `web.xml` file in the WEB-INF directory of the war.

> Modify the `web.xml` file of this war like written in chapter "`web.xml` file" of the [UserGuide](UserGuide.md).

> Make sure that the javamelody.jar file is only in the EAR/lib directory, and not also in the WAR/WEB-INF/lib directory, otherwise you might get issues with class loading (recorded data might be in a different class loader than the one of the monitoring web app.)
> Note that if you have class loading issues with an ear, the use of a single war instead of an ear could help (and with JavaEE 6, you can even use EJBs in a single war).

### 3. First results ###

> You can now view the monitoring: deploy your ear and open the following page in a web navigator after starting the server:

> `http://<host>/<context>/monitoring`

> where `<host>` is the name of the server where the application is deployed, followed possibly by the port (for example localhost:8080)
> and where `<context>` is the name of the context of the webapp which you have configured in the ear.

> Then you can complete the settings as below according to your needs.

### 4 to 17. Other settings (optional parameters, JDBC, EJB3, etc) ###

> To complete the settings, just follow chapters 4 to 17 of the [UserGuide](UserGuide.md).
> As you use an ear file, you may be interested by the chapter "Business façades (ejb-jar.xml file if EJB3)" for example.


## Optional centralization server setup ##

> A centralized collect server can be used optionally.
> This server can centralize in a same user interface and with a unique storage
> the monitoring of several applications (QA and production for example)
> and / or the monitoring of an application with several instances of servers (a cluster for example).
> So the reports, the storage and the charts are moved out of the monitored application to the collect server.

> Please note that the centralized collect server with javamelody.war only works if the [monitoring](UserGuide.md) already works with javamelody.jar in the target application at `http://<host>:<port>/<context>/monitoring`. An application must not be monitored from two collector servers.

### 1. War file of the webapp of monitoring ###

> Copy the javamelody.war file, located at the root of the supplied javamelody.zip file, to the centralized server.
> The javamelody.war file should be in general of the same version of monitoring
> as the version of the javamelody.jar file in the monitored application.

### 2. Deployment of the webapp of monitoring ###

> Note : An alternative which does not need an application server is described in the following chapter.

> Deploy the javamelody.war file in the application server of the centralized server.
> If it is Tomcat, you can write an xml context file named 'javamelody.xml' in the directory
> conf/Catalina/localhost of Tomcat as in the following example:

```
	<?xml version="1.0" encoding="UTF-8" ?>
	<Context docBase="<pathto>/javamelody.war" path="javamelody" reloadable="false" >
		<Parameter name='javamelody.resolution-seconds' value='120' override='false'/>
	</Context>
```

> The optional parameters `resolution-seconds`, `storage-directory`, `log`, `warning-threshold-millis`,
> `severe-threshold-millis` and `allowed-addr-pattern` can be added in the xml context file of the collect server with the prefix `javamelody`. They can also be added as system properties with the prefix `javamelody`.

> They have the same effects on the collect server than those of a monitored application as written in the [UserGuide](UserGuide.md). In particular the parameter `resolution-seconds` defines the period of calls to urls of applications from collect server and the resolution of charts in the monitoring. Logs such as connect exceptions are printed in the standard output of the server, and to have more logs you can add the system property `-Djavamelody.log=true`

> The collect server can send weekly, daily or monthly reports by mail for each monitored application. For this, use in the collect server (in Tomcat context for example) exactly the same mail session, the same parameters and the same jar files as those in [Weekly, daily or monthly reports by mail](UserGuide#14._Weekly,_daily_or_monthly_reports_by_mail.md).

> If xml format is desired instead of java serialization as transport format between a collect server and monitored applications, a parameter `transport-format` which is specific to the collect server can be added with 'xml' for value. The java serialization is the transport format by default and it is recommended for best performances (75% more performance for response time, according to this [benchmark](http://code.google.com/p/thrift-protobuf-compare/wiki/Benchmarking?ts=1237772203&updated=Benchmarking)). The xml transport format needs a dependency on libraries xstream (BSD) and xpp3 (Public Domain) in the monitored webapps.

### 3. Simpler alternative of deployment of the webapp of monitoring ###

> As an alternative to the deployment described in the last chapter for the webapp of monitoring in a java application
> server, it is possible to launch the [winstone](http://winstone.sourceforge.net) (LGPL) servlet container
> already included in the javamelody.war file.
> You only need to use the launch command "java -jar javamelody.war" and to specify in system properties each
> of the monitoring parameters detailed above.

> Examples:

```
	java -jar javamelody.war
	java -Djavamelody.resolution-seconds=120 -jar javamelody.war
```

> The settings of http ports, server mode, memory and logs can be done like this:

```
	java -server -Xmx128m -jar javamelody.war --httpPort=8080 --ajp13Port=8009 2>&1 >>javamelody.log
```

> Other parameters exist for the servlet container. You can read them with:

```
	java -jar javamelody.war --usage
```

> On Linux and Unix, it is possible to launch this server as a daemon (in background) and with logs like this
> ([wikipedia](http://fr.wikipedia.org/wiki/Nohup)):

```
	nohup java -server -jar javamelody.war 0</dev/null 2>&1 >>javamelody.log &
```


### 4. Results with a collect server ###

> To view the monitoring: open the following page in a web navigator after starting the servers:

> `http://<host>/javamelody`

> where `<host>` is the name of the collect server, followed possibly by the port (for example localhost:8080)
> and where 'javamelody' is the name of the context of the webapp as the name of the 'javamelody.xml' file.

> With links by application in reports, you can choose the application that you want to monitor.


> In the web page, you can add an application to monitor and its access url
> (for example "`http://<host_qa>/myapp/`" for "qa" and "`http://<host_production>/myapp/`" for "production").
> If an application to monitor is deployed on several instances of servers (in a cluster or in a farm),
> urls should be separated by ','
> (for example "`http://<host1>/myapp/,http://<host2>/myapp/`" for "cluster"). Of course, you can otherwise add each instance individually.
> Note: it is not possible to monitor one application in several collect servers or twice in one collect server.

> In order for the collect server to monitor an application, the monitoring should be setup in this application
> as written at the start of this chapter.
> When the monitoring is setup in the application, it is not compulsory to include the `jrobin-*.jar` file in the
> WEB-INF/lib directory of the application, as only the collect server will handle the charts for this application.


### 5. Security with a collect server ###

> And with this setup again, it is possible to restrict access to the monitoring of the application only to the
> collect server by its ip address, forbidding access to the monitoring of the application to any others.
> For example, if the monitored application is on the same server as the collect server, the following parameter
> can be added in the `web.xml` file of the monitored application to restrict access and to allow only the local collect server:

```
	<init-param>
		<param-name>allowed-addr-pattern</param-name>
		<param-value>127\.0\.0\.1</param-value>
	</init-param>
```

> If the monitored application is a Hudson, Jenkins, JIRA, Confluence or Bamboo server, the system property -Djavamelody.plugin-authentication-disabled=true can be added to the monitored server in order to disable authentication of the monitoring page in the javamelody plugin and to be able to add the server to a centralized collect server. (A system property like -Djavamelody.allowed-addr-pattern=127\.0\.0\.1 can also be added with the ip address of the collect server.)

> Instead of using `allowed-addr-pattern`, for example when you don't know upfront the ip address of the collect server, you may want to secure access with http basic authentication (username and password) in the monitored application. For that, add the following in the `web.xml` file of the monitored application:

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
        </security-constraint>
```

> The realm and the user used by the collect server must be defined in the monitored application server, and the user must have the "monitoring" role to have access. For example, if tomcat is used with the default realm in the monitored application server, modify the content of the conf/tomcat-users.xml file as follows:

```
        <?xml version='1.0' encoding='utf-8'?>
        <tomcat-users>
             <role rolename="monitoring"/>
             <user username="monitoring" password="monitoring" roles="monitoring"/>
        </tomcat-users>
```

> Or, if you want BASIC authentication with username and password, but do no want to use a realm and "security-constraint" in web.xml, you can add the parameter "authorized-users" in web.xml, in context or in system properties like the other [javamelody parameters](UserGuide#6._Optional_parameters.md). For example with a system property -Djavamelody.authorized-users=user1:pwd1,user2:pwd2 (_since v1.53_)

> Then, when you add the monitored application in the collector server, define the username and the password in the URL. For example, the URL of the monitored application, as given to the collect server, could be:
> http://myusername:mypassword@myhost:8080/mywebapp

> To restrict access to the reports in the collector server, you can also set the -Djavamelody.authorized-users=user1:pwd1,user2:pwd2 system property on the collector server (_since v1.53_).

> Furthermore, if you do not want the people having access to the collect server to be able to add or remove applications to monitor, you can forbid the writing to the file "applications.properties" which contains the list of the applications to monitor. This file is located in the storage directory, "temp/javamelody/applications.properties" for example. The user of the OS running the java process of the collect server must be allowed to read the file but without being allowed to write it. Only you, by using "root" for example, will be able by editing the file to add or to remove applications as the collect server would do. And in this case, the links "add an application" and "remove an application" will not be displayed in the collect server.

> See also the comment by Chris at the bottom of this page.


### 6. If reverse proxy ###

> The reports use a Cookie, for example to know the currently displayed application. So if you use Apache as reverse proxy in front of the collector server and if URL are rewritten, the cookie path will not match the path used by the browser. And for example, the charts will not be displayed for the right application.

> To fix the cookie path, you may use [mod\_proxy's top ProxyPassReverseCookiePath directive](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html#proxypassreversecookiepath)

## Business facades (without EJB3 and without Spring and without Guice) ##

> If the application to monitor contains some business façades with Java interfaces, a counter can be created for statistics of execution of methods on these façades even if these interfaces are neither EJB3 nor Spring nor Guice.

> First, if these façades are EJB3, Spring or Guice, then it is simpler to use the configuration described in the user guide. Otherwise let's say that a business façade has an interface with an implementation, which is instanciated in a factory for example as below:

```
        public interface MyFacade { ... }
        
        public class MyFacadeImpl implements MyFacade { ... }

        public class Factory {
                public static MyFacade createMyFacade() {
                        final MyFacade myFacade = new MyFacadeImpl();
                        return myFacade;
                }
        }
```

> So it is a standard Java interface (ie POJI or Plain Old Java Interface). Then you just need to modify the code of the instanciation like this:

```
        public class Factory {
                public static MyFacade createMyFacade() {
                        final MyFacade myFacade = net.bull.javamelody.MonitoringProxy.createProxy(new MyFacadeImpl());
                        return myFacade;
                }
        }
```

> As pre-requisites for this, the jar of javamelody must be available in the classpath used to compile sources and façades must have interfaces and not only implementations.

> If the name displayed by the monitoring for the class does not please you,
> you can name the proxy like this:

```
	public class Factory {
		public static MyFacade createMyFacade() {
			final MyFacade myFacade = net.bull.javamelody.MonitoringProxy.createProxy(new MyFacadeImpl(), "my business use case");
			return myFacade;
		}
	}
```


## Enable Hotspots detection ##

When enabled, the Hotspots screen displays CPU hotspots in executed methods for all the JVM (_since v1.47_).

The overhead of hotspots detection is low: it is based on sampling of stack-traces of threads, without instrumentation. And it is made to be always active if enabled. (It is currently not enabled by default. It may be enabled by default in the future.)

Three parameters can be defined in web.xml, in context or in system properties like the other [parameters](UserGuide#6._Optional_parameters.md):

  * "sampling-seconds" to enable the hotspots sampling and to define its period. A period of 10 seconds can be recommended to have the lowest overhead, but then a few hours may be needed to have significant results for a webapp under real use. If you don't mind having a bit more overhead or want a result faster in test, you can use a value of 1 or 0.1 in second for this parameter.
  * "sampling-excluded-packages" to change the list of the excluded packages ("java,sun,com.sun,javax,org.apache,org.hibernate,oracle,org.postgresql,org.eclipse" by default)
  * or "sampling-included-packages" for a white list of included package

## JPA monitoring ##
Inspired by [Apache Sirona](http://sirona.incubator.apache.org/)

If you use JPA with a persistence.xml file, you can monitor main calls on the JPA [EntityManager](http://docs.oracle.com/javaee/7/api/javax/persistence/EntityManager.html) (_since v1.50_).

To enable the JPA monitoring with JPA statistics in the reports, set `net.bull.javamelody.JpaPersistence` as JPA Provider in your persistence.xml file. For example:
```
	<?xml version="1.0" encoding="UTF-8"?>
	<persistence version="2.0"
				xmlns="http://java.sun.com/xml/ns/persistence"
				xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
				xsi:schemaLocation="http://java.sun.com/xml/ns/persistence
				http://java.sun.com/xml/ns/persistence/persistence_2_0.xsd">
		<persistence-unit name="my-unit">
			<provider>net.bull.javamelody.JpaPersistence</provider>
			...
		</persistence-unit>
	</persistence>
```

If you have in your environment a single "real" JPA provider it should be found automatically.
But if that's not the case or if you want to force the implementation set the property `net.bull.javamelody.jpa.provider` to the real implementation you want. For example:
```
	<?xml version="1.0" encoding="UTF-8"?>
	<persistence version="2.0"
				xmlns="http://java.sun.com/xml/ns/persistence"
				xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
				xsi:schemaLocation="http://java.sun.com/xml/ns/persistence
				http://java.sun.com/xml/ns/persistence/persistence_2_0.xsd">
		<persistence-unit name="my-unit">
			<provider>net.bull.javamelody.JpaPersistence</provider>
			<properties>
				<property name="net.bull.javamelody.jpa.provider" value="org.hibernate.jpa.HibernatePersistenceProvider" />
				...
			</properties>
			...
		</persistence-unit>
	</persistence>
```

## Alternative for monitoring of sql requests ##

> If for any reason you can't monitor sql requests with a jdbc datasource or with a jdbc driver
> as said in the JDBC chapter of the user guide, and if you use Hibernate then there is an
> alternative for monitoring of sql requests: define the Hibernate property
> "`hibernate.jdbc.factory_class`" with the value "`net.bull.javamelody.HibernateBatcherFactory`".
> It will enable the display of statistics of sql requests and the display of active jdbc connections.

> For example, if you use an "hibernate.cfg.xml" file, add the following with the other Hibernate
> properties:

```
	<property name="hibernate.jdbc.factory_class">net.bull.javamelody.HibernateBatcherFactory</property>
```

## Monitoring of sql requests and of jdbc connections in GlassFish v3+ ##

If you use a JDBC DataSource from JNDI with GlassFish v3 or more, the SQL requests and the JDBC connections are probably not monitored.

To monitor them, it is needed to do the following, for each JDBC DataSource:
  * It is supposed that the DataSource is for example currently named "jdbc/MyDataSource" in GlassFish
  * Download [javamelody-objectfactory.jar](http://javamelody.googlecode.com/files/javamelody-objectfactory.jar) and copy this file in the lib directory of your GlassFish server
  * In the GlassFish admin console, rename your JDBC Resource (DataSource) from "jdbc/MyDataSource" to "jdbc/MyDataSource\_uncached" for example
  * Also in the GlassFish admin console, create a JNDI custom resource like this, without forgetting then "jndi-ref" property:

<a href='http://javamelody.googlecode.com/svn/wiki/images/GlassFish_create-custom-resource.png'><img src='http://javamelody.googlecode.com/svn/wiki/images/GlassFish_create-custom-resource.png' alt='New Custom Resource' width='80%' />
</a>

or use the following asadmin command

` asadmin create-custom-resource --restype javax.naming.spi.ObjectFactory --factoryclass javamelody.CachedObjectFactory --property jndi-ref=jdbc/MyDataSource_uncached jdbc/MyDataSource `

or you can use the ` asadmin add-resources /path/to/sample-resource.xml ` command with the file [sample-resource.xml](http://javamelody.googlecode.com/svn/trunk/javamelody-core/src/main/objectfactory/javamelody/sample-resource.xml)

  * Then in the details of the resource in the GlassFish admin console, define the target of the custom resource if the monitored webapp is deployed on another target instance than the Admin Server
  * Test: Webapp uses "jdbc/MyDataSource" as before to lookup from JNDI (and now it is the custom resource referencing "jdbc/MyDataSource\_uncached"). After some usage of the webapp, the statistics of the SQL requests should be displayed in the monitoring report.


## Usage of JavaMelody in JonAS 5 (which uses OSGI) ##

JonAS 5 uses OSGI and so you need to add a configuration of felix in JonAS to use JavaMelody.

Copy the content of the latest [felix-config.properties](http://websvn.ow2.org/filedetails.php?repname=jonas&path=%2Fjonas%2Fbranches%2Fjonas_5_1%2Fjonas%2Fmodules%2Ftools%2Flaunchers%2Ffelix-launcher%2Fsrc%2Fmain%2Fresources%2Forg%2Fow2%2Fjonas%2Flauncher%2Ffelix%2Fdefault-config.properties) for JonAS in a file named "felix-config.properties" and written in the "conf" directory of your JonAS server.
If the content of this latest file does not suit your JonAS version, you can find the "felix-config.properties" file for your version in the felix jar file of your server.

In your "felix-config.properties" file, add the following system packages after the existing "org.osgi.framework.system.packages" declaration:
> "com.sun.management" and "sun.nio.ch".

For example:

```
org.osgi.framework.system.packages com.sun.management; \
				   sun.nio.ch; \
				   org.osgi.framework; version=1.5.0, \
```

Then add a system property in your launch file: -Djonas.felix.configuration.file=$JONAS\_BASE/conf/felix-config.properties


## Deployment on Tomcat without modification of monitored webapps (beta) ##

> If there is one or several webapp(s) to monitor which are deployed on Tomcat 6,
> it is possible to monitor this or these webapp(s) without modification of this or these webapp(s).
> That is to say without modification of the war file or of the directory of these webapps.

> For this, copy the files `javamelody.jar` and `jrobin-x.jar` and optionally `itext-x.jar` in the lib directory of Tomcat 6 (and not in the WEB-INF/lib directories of the webapps).

> Then add the following lines in the web.xml file of the conf directory of Tomcat (and not in the WEB-INF/web.xml files of the webapps).

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

> In this conf/web.xml file of tomcat, some parameters can be added as written in the [user guide](UserGuide.md).

> So and without modification of the webapps, all webapps deployed in this Tomcat instance are monitored
> (and it is then not possible to choose which, otherwise than with the url-exclude-pattern parameter described in the [user guide](UserGuide.md)).
> This technique is specific to Tomcat 6 and does not apply to other JavaEE implementations.

> The `ClassLoader` in application servers is a complex subject. In consequence :

  * This feature is considered as beta for now

  * It works only in Tomcat 6 or 7 (Tomcat 5.5 is not supported)

  * Hot reployment is not supported when using this technique

  * It does not seem advisable to use the monitoring by Spring AOP in this case


## Embedding JavaMelody in a standalone app ##

> In general, JavaMelody is installed in a webapp with a server such as Tomcat. But if you want to monitor a standalone application (non webapp and no server), you can embed JavaMelody with a Jetty Server in your application. For that:

  * add Jetty, and JavaMelody with JRobin dependencies in you application. If you use Maven, you can copy dependencies from this [pom.xml](https://code.google.com/p/javamelody/source/browse/trunk/javamelody-for-standalone/pom.xml).

  * Then, copy this example of [EmbeddedServer](https://code.google.com/p/javamelody/source/browse/trunk/javamelody-for-standalone/src/main/java/net/bull/javamelody/EmbeddedServer.java) class in your application.

  * And you can start the server [like this](https://code.google.com/p/javamelody/source/browse/trunk/javamelody-for-standalone/src/main/java/example/Main.java). For example, add the following in the main class of your application:

```
                final Map<Parameter, String> parameters = new HashMap<>();
                // you can add basic auth:
                parameters.put(Parameter.AUTHORIZED_USERS, "admin:pwd");
                // you can change the default storage directory:
                parameters.put(Parameter.STORAGE_DIRECTORY, "/tmp/javamelody");
                // you can enable hotspots sampling with a period of 1 second:
                parameters.put(Parameter.SAMPLING_SECONDS, "1.0");

                // set the path of the reports:
                parameters.put(Parameter.MONITORING_PATH, "/");
                // start the embedded http server with javamelody
                EmbeddedServer.start(8080, parameters);
```

> When running you application with embedded JavaMelody, open http://localhost:8080 to browse the monitoring reports.

## Debugging logs ##

> The debugging logs, such as javamelody initialization, are displayed at the bottom of the html report.

> Logs for debugging are also available using either logback if found, or log4j if found or
> java.util.logging otherwise. The log messages for debugging are written using the category
> "net.bull.javamelody" and the level "debug".

> Moreover, some stack-traces of unusual exceptions are logged using logback or log4j or
> java.util.logging, in the same category with the level "info" or "warn".

> If the logback library is available in your monitored webapp, you can display debugging logs
> by adding or modifying the "logback.xml" file in your "WEB-INF/classes" directory. For example:

```
	<?xml version="1.0" encoding="UTF-8" ?>
	<configuration>
		<appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
			<encoder>
				<pattern>%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n
				</pattern>
			</encoder>
		</appender>
	
		<!-- http requests log -->
		<logger name="javamelody">
			<level value="info" />
		</logger>
		
		<!-- debugging log -->
		<logger name="net.bull.javamelody">
			<level value="debug" />
		</logger>
	
		<root level="debug">
			<appender-ref ref="STDOUT" />
		</root>
	</configuration>
```

> Or if the log4j library is available in your monitored webapp, you can display debugging logs
> by adding or modifying the "log4j.xml" file in your "WEB-INF/classes" directory. For example:

```
	<?xml version="1.0" encoding="UTF-8"?>
	<!DOCTYPE log4j:configuration SYSTEM "log4j.dtd">
	<log4j:configuration xmlns:log4j="http://jakarta.apache.org/log4j/" debug="false">
		<appender name="CONSOLE" class="org.apache.log4j.ConsoleAppender">
			<param name="Threshold" value="DEBUG" />
			<layout class="org.apache.log4j.PatternLayout">
				<param name="ConversionPattern" value="%d{ABSOLUTE} %-5p [%c] %m%n" />
			</layout>
		</appender>
	
		<!-- http requests log  -->
		<category name="javamelody">
			<priority value="info" />
		</category>
		
		<!-- debugging log -->
		<category name="net.bull.javamelody">
			<priority value="debug" />
		</category>
		
		<root>
			<appender-ref ref="CONSOLE" />
		</root>
	</log4j:configuration>
```

> Here is an example of debugging logs:

```
11:11:00.120 [main] DEBUG net.bull.javamelody - JavaMelody listener init started
11:11:00.151 [main] DEBUG net.bull.javamelody - datasources found in JNDI: [java:comp/env/jdbc/TestDB2, java:comp/env/jdbc/TestDB]
11:11:00.167 [main] DEBUG net.bull.javamelody - datasource rebinded: java:comp/env/jdbc/TestDB2 from class org.apache.tomcat.dbcp.dbcp.BasicDataSource to class $Proxy0
11:11:00.167 [main] DEBUG net.bull.javamelody - datasource rebinded: java:comp/env/jdbc/TestDB from class org.apache.tomcat.dbcp.dbcp.BasicDataSource to class $Proxy0
11:11:00.167 [main] DEBUG net.bull.javamelody - JavaMelody listener init done
11:11:00.182 [main] DEBUG net.bull.javamelody - JavaMelody filter init started
11:11:00.182 [main] DEBUG net.bull.javamelody - OS: Windows XP Service Pack 3, x86/32
11:11:00.182 [main] DEBUG net.bull.javamelody - Java: Java(TM) SE Runtime Environment, 1.6.0_21-b06
11:11:00.182 [main] DEBUG net.bull.javamelody - Server: Apache Tomcat/6.0.26
11:11:00.182 [main] DEBUG net.bull.javamelody - Webapp context: /test
11:11:00.182 [main] DEBUG net.bull.javamelody - JavaMelody version: 1.19.0-SNAPSHOT
11:11:00.182 [main] DEBUG net.bull.javamelody - Host: xxx@xxx
11:11:00.182 [main] DEBUG net.bull.javamelody - parameter defined: resolution-seconds=60
11:11:00.182 [main] DEBUG net.bull.javamelody - parameter defined: log=true
11:11:00.182 [main] DEBUG net.bull.javamelody - parameter defined: system-actions-enabled=true
11:11:00.182 [main] DEBUG net.bull.javamelody - parameter defined: mail-session=mail/Session
11:11:00.182 [main] DEBUG net.bull.javamelody - parameter defined: admin-emails=xxx
11:11:00.182 [main] DEBUG net.bull.javamelody - parameter defined: mail-periods=day,week,month
11:11:00.292 [main] DEBUG net.bull.javamelody - log listeners initialized
11:11:00.307 [main] DEBUG net.bull.javamelody - datasources found in JNDI: [java:comp/env/jdbc/TestDB2, java:comp/env/jdbc/TestDB]
11:11:00,386 INFO  [org.quartz.simpl.SimpleThreadPool] Job execution threads will use class loader of thread: main
11:11:00,432 INFO  [org.quartz.core.QuartzScheduler] Quartz Scheduler v.1.5.2 created.
11:11:00,432 INFO  [org.quartz.simpl.RAMJobStore] RAMJobStore initialized.
11:11:00,432 INFO  [org.quartz.impl.StdSchedulerFactory] Quartz scheduler 'DefaultQuartzScheduler' initialized from default resource file in Quartz package: 'quartz.properties'
11:11:00,432 INFO  [org.quartz.impl.StdSchedulerFactory] Quartz scheduler version: 1.5.2
11:11:00.432 [main] DEBUG net.bull.javamelody - job global listener initialized
11:11:00.432 [main] DEBUG net.bull.javamelody - counters initialized
11:11:00.495 [main] DEBUG net.bull.javamelody - counters data read from files in xxx\apache-tomcat-6.0.26\temp\javamelody\test_xxx
11:11:00.511 [main] DEBUG net.bull.javamelody - collect task scheduled every 60s
11:11:00.776 [main] DEBUG net.bull.javamelody - first collect of data done
11:11:00.792 [main] DEBUG net.bull.javamelody - mail report for the day period scheduled with next execution date at Wed Aug 18 00:00:00 CEST 2010
11:11:00.792 [main] DEBUG net.bull.javamelody - mail report for the week period scheduled with next execution date at Sun Aug 22 00:00:00 CEST 2010
11:11:00.792 [main] DEBUG net.bull.javamelody - mail report for the month period scheduled with next execution date at Wed Sep 01 00:00:00 CEST 2010
11:11:00.792 [main] DEBUG net.bull.javamelody - mail reports scheduled for xxx
11:11:00.792 [main] DEBUG net.bull.javamelody - JavaMelody filter init done
```

## Clearing all statistics and all graphs ##

> The storage files of statistics and of graphs are stored in the temporary directory of the server, unless if you have defined the "storage-directory" parameter. And there is a sub directory for each application.

> For example, with tomcat it is the directory `<TOMCAT_HOME>/temp/javamelody/`.
> With some other servers on linux, it is the directory `/tmp/javamelody/`.
> And for the Hudson / Jenkins plugin, it is the directory `<HUDSON_HOME>/javamelody/`.

> To clear all statistics and all graphs:

  * Stop the server

  * Delete the storage directory : javamelody in the temporary directory of the server

  * Restart the server. You can then open the report of the monitoring which is now empty.

> But if you want to clear only the statistics, delete the `*.gz` files in the subdirectories of the storage directory.
> And if you want to clear only the graphs, delete the `*.rrd` files in the subdirectories of the storage directory.

## Custom reports ##

You can include links to your custom reports in the floating menu on the right of the report (_since v1.50_).

For that, add a parameter named "custom-reports" like the other [javamelody parameters](UserGuide#6._Optional_parameters.md). In the value of this parameter, put the list of names of your custom reports separated with commas. Then for each custom report, add a parameter with the same name and its path as value.

For example:
```
		<init-param>
			<param-name>custom-reports</param-name>
			<param-value>Send feedback,My custom report</param-value>
		</init-param>
		<init-param>
			<param-name>Send feedback</param-name>
			<param-value>https://groups.google.com/forum/#!forum/javamelody</param-value>
		</init-param>
		<init-param>
			<param-name>My custom report</param-name>
			<param-value>/WEB-INF/pages/myCustomReport.jsp</param-value>
		</init-param>
```

If the path starts with '/', it will be considered as a JSP (or a servlet) inside the webapp. Otherwise it will be considered as an URL (http://example.com).
As example of JSP, you can add the file "/WEB-INF/pages/myCustomReport.jsp" in your webapp, with a content like [this one](https://code.google.com/p/javamelody/source/browse/trunk/javamelody-core/src/test/test-webapp/WEB-INF/pages/myCustomReport.jsp).
This [example can be seen in the demo](http://demo.javamelody.cloudbees.net/monitoring?report=My+custom+report).

## Customizing styles, icons and other resources in the html reports ##

> It can be helpful to customize styles, icons, effects and javascript or help files in the html reports (pdf reports excluded).
> The reports can be adapted to the styles of some enterprise applications with the CSS file.
> For example, add the following, before the monitoring filter, in the web.xml file of your webapp, in order to use your own css or icons:

```
 <filter>
	<filter-name>customResourceFilter</filter-name>
	<filter-class>net.bull.javamelody.CustomResourceFilter</filter-class>
	<init-param>
		<param-name>monitoring.css</param-name>
		<param-value>/customMonitoring.css</param-value>
	</init-param>
	<init-param>
		<param-name>bullets/green.png</param-name>
		<param-value>/static/bullets/red.png</param-value>
	</init-param>
 </filter>
 <filter-mapping>
	<filter-name>customResourceFilter</filter-name>
	<url-pattern>/monitoring</url-pattern>
 </filter-mapping>
```

> Then add files "customMonitoring.css" and "static/bullets/red.png" at the root of the web content in your webapp.
> You can replace every web resource in this directory and its sub-directories: "[src/main/resources/net/bull/javamelody/resource](https://code.google.com/p/javamelody/source/browse/#svn%2Ftrunk%2Fjavamelody-core%2Fsrc%2Fmain%2Fresources%2Fnet%2Fbull%2Fjavamelody%2Fresource)".

> This configuration is for a monitored webapp. For the optional collector server, see [issue 414](https://code.google.com/p/javamelody/issues/detail?id=414) instead.

## Using a servlet to display the monitoring reports ##

> The MonitoringFilter documented in the [user guide](UserGuide#2._web.xml_file.md) is enough to display reports for the "/monitoring" url. You can also change that url with the [monitoring-path parameter](UserGuide#6._Optional_parameters.md).

> If for some reason, you want to use a servlet to display the reports, then such a servlet already exists and you can use it by adding the following in the WEB-INF/web.xml file of your webapp:

```
	<servlet>
		<servlet-name>monitoringServlet</servlet-name>
		<servlet-class>net.bull.javamelody.ReportServlet</servlet-class>
	</servlet>
	<servlet-mapping>
		<servlet-name>monitoringServlet</servlet-name>
		<url-pattern>/admin/monitoring</url-pattern>
	</servlet-mapping>
```

> You can change the url-pattern above to anything you want, provided that you use the same path in your browser to access the reports.

## Report written before last shutdown ##

> Sometimes a server is stopped in emergency or when there is a problem for example with the used memory.
> And perhaps, you did not thought of saving the JavaMelody report to have precise figures about the state of the server at this moment.

> But don't worry, when the application is undeployed, JavaMelody writes a file called `last_shutdown.html` in the same storage directory as explained in the previous chapter. It is the JavaMelody report in html format, and it contains the statistics for the current day and the system information (such as the used memory) for the current time just before the application is undeployed, but it does not contain charts.

## Format of RRD files ##

> Values of charts are stored in RRD (Round Robin Database) format.
> Many tools exist for this format like [RRDtool](http://oss.oetiker.ch/rrdtool/),
> close relative of [MRTG](http://oss.oetiker.ch/mrtg/), or [JRobin](http://www.jrobin.org/) in Java.

## Compilation and development ##

> See [DevGuide](DevGuide.md)