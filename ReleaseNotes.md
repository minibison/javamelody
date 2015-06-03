### Next release (trunk) ###

  * 

### 1.56.0 ###

  * fix [issue 476](https://code.google.com/p/javamelody/issues/detail?id=476): ClassLoader leak when using the reports and then hot-redeploy with Tomcat 7.0.42 or later
  * fix [issue 475](https://code.google.com/p/javamelody/issues/detail?id=475): IllegalAccessException when creating Jdbc Proxy (Java 8)
  * fix [issue 473](https://code.google.com/p/javamelody/issues/detail?id=473): Basic Authentication and Sessions
  * fix [issue 460](https://code.google.com/p/javamelody/issues/detail?id=460): In WildFly 8.2, "ClassNotFoundException: Can't find a delegate" when monitoring JPA
  * fix [issue 477](https://code.google.com/p/javamelody/issues/detail?id=477): In the processes list, CPU and Command are sometimes in the wrong column
  * added: MULTILINE & DOTALL flags for transform-patterns regexps ([issue 474](https://code.google.com/p/javamelody/issues/detail?id=474), thanks to _Michal Bergmann_).
  * doc added: Example of [embedding JavaMelody in a standalone application](UserGuideAdvanced#Embedding_in_a_standalone_app.md) (non webapp), thanks to _Philip Zeyliger_ and _let4time_
  * doc updated: Examples of web.xml and of context-param updated in the [user's guide](UserGuide#2._web.xml_file.md) to be sure that parameters are not ignored in some case of webapp v3.1 with JDK8 ([issue 459](https://code.google.com/p/javamelody/issues/detail?id=459)).
  * Sources from [SVN](http://javamelody.googlecode.com/svn/trunk/javamelody-core/) are now mirrored in Github: https://github.com/evernat/javamelody. The Github mirror is synchronized once a day by this Jenkins [job](https://javamelody.ci.cloudbees.com/job/svn_to_git_mirroring/). (This mirror does not include [Jenkins](https://github.com/jenkinsci/monitoring-plugin), [Liferay](https://github.com/evernat/liferay-javamelody), [Grails](https://github.com/evernat/grails-melody-plugin) and [Sonar](https://github.com/evernat/sonar-javamelody) plugins.)

  * Contributors to code can fork the github mirror [repository](https://github.com/evernat/javamelody) and submit [pull requests](https://github.com/evernat/javamelody/pulls): the diff of the PR will be merged into SVN. Or contributors can still checkout from [SVN](http://javamelody.googlecode.com/svn/trunk/) and submit patches in [issues](https://code.google.com/p/javamelody/issues/list) as before.


### 1.55.0 ###

  * fix ~~[issue 104](https://code.google.com/p/javamelody/issues/detail?id=104#c14)~~ again, for recent TomEE versions (thanks to _Frederic Cornuau_)
  * fix [issue 436](https://code.google.com/p/javamelody/issues/detail?id=436): implement Servlet 3.1 new methods (JavaEE 7), in FilterServletOutputStream and others
  * fix [issue 453](https://code.google.com/p/javamelody/issues/detail?id=453): Chinese translation for heap dump (thanks to _chuxuebao_)
  * fix [issue 455](https://code.google.com/p/javamelody/issues/detail?id=455): HTTP-401 / WWW-Authenticate wrongly reported as HTTP error

### 1.54.0 ###

  * fix [issue 440](https://code.google.com/p/javamelody/issues/detail?id=440): Not able to start Desktop version.
  * fix broken link to the ehcache documentation ([revision 3972](https://code.google.com/p/javamelody/source/detail?r=3972))
  * fix: In the [Jenkins plugin](https://wiki.jenkins-ci.org/display/JENKINS/Monitoring), the monitoring reports of a slave didn't work if its name contains a space.
  * improved: In the optional collector server, when the monitoring-path parameter such as /admin/monitoring is used in the monitored webapp(s), support the same parameter for calling the webapp(s) from the collector server, for example -Djavamelody.monitoring-path=/admin/monitoring ([issue 442](https://code.google.com/p/javamelody/issues/detail?id=442), thanks to _Dimitris Mouchritsas & Manolis Pitarokilis_)
  * improved: In the optional collect server, upgrade the embedded Winstone server.

### 1.53.0 ###

  * fix: [XSS](http://en.wikipedia.org/wiki/Cross-site_scripting) vulnerability in the reports
  * fix: in v1.52.0 with Tomcat, graphs of bytes received/sent and of Tomcat active threads were not displayed anymore in "Other charts" ([revision 3908](https://code.google.com/p/javamelody/source/detail?r=3908)). This was unfortunately caused by the fix of [issue 406](https://code.google.com/p/javamelody/issues/detail?id=406).
  * fix [issue 342](https://code.google.com/p/javamelody/issues/detail?id=342): In JIRA logs since JIRA v6.0.8, "RuntimeException converting item 'net.bull.javamelody:net.bull.javamelody.jira-web-item' to Simple link"
  * fix [issue 439](https://code.google.com/p/javamelody/issues/detail?id=439): Display Linux version in "System informations", and not "Linux unknown"
  * improved: Portuguese translation ([revision 3885](https://code.google.com/p/javamelody/source/detail?r=3885), thanks to _Fernando Boaglio_)
  * added: new parameter "sampling-included-packages" for a white list in [cpu hotspots](UserGuideAdvanced#Enable_Hotspots_detection.md), instead of using the "sampling-excluded-packages" parameter ([issue 424](https://code.google.com/p/javamelody/issues/detail?id=424), thanks to _alf.hogemark_)
  * added: new parameter "authorized-users" for **BASIC authentication** with username and password to access the reports in the monitored webapp or in the optional collect server ([revision 3890](https://code.google.com/p/javamelody/source/detail?r=3890), [revision 3914](https://code.google.com/p/javamelody/source/detail?r=3914), thanks to _Benjamin Durand_).

If you want BASIC auth but do no want to [use a realm and "security-constraint" in web.xml](UserGuide#16._Security.md), you can add the parameter "**authorized-users**" in web.xml, in context or in system properties like the other [javamelody parameters](UserGuide#6._Optional_parameters.md). For example in your WEB-INF/web.xml file:
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

### 1.52.0 ###

  * fix [issue 406](https://code.google.com/p/javamelody/issues/detail?id=406): Slow GUI with JBoss EAP 6.2 (only since 6.2)
  * fix [issue 415](https://code.google.com/p/javamelody/issues/detail?id=415): Periodic stack trace in Tomcat 8 logs 'java.lang.IllegalStateException: The resources may not be accessed if they are not currently started'
  * fix [issue 423](https://code.google.com/p/javamelody/issues/detail?id=423): Link to DataSource Reference Returns HTTP 404
  * fix in the [Liferay plugin](https://github.com/evernat/liferay-javamelody), the monitoring page works after deploy, but is not found (404) after restart of Liferay ([commit 78618d3](https://github.com/evernat/liferay-javamelody/commit/78618d3269bbaa319672f686a0d57ba7b251029b)). This was caused by interaction between web-fragment.xml and Tomcat 7 inside Liferay.
  * fix [issue 377](https://code.google.com/p/javamelody/issues/detail?id=377): in JIRA, Confluence: Incorrect Installation Instructions on Atlassian Marketplace
  * improved: reduce the number of RRD files created in some conditions, by not creating a RRD file of mean times if a request is called only once ([revision 3836](https://code.google.com/p/javamelody/source/detail?r=3836)). Otherwise, obsolete RRD files are automatically deleted after 3 months like before.
  * added: links "View in a new page" below the tables of threads and of current requests ([revision 3839](https://code.google.com/p/javamelody/source/detail?r=3839)). For example when the app is under load, the new pages make it easy to refresh at will the details of threads or of the current requests to see if states change or not, without refreshing all the main page.
  * added: logout action in the menu on the right of the main report, if there is a http session ([revision 3859](https://code.google.com/p/javamelody/source/detail?r=3859))
  * In the [Jenkins monitoring plugin](https://wiki.jenkins-ci.org/display/JENKINS/Monitoring), upgraded mininum Jenkins version to 1.509.3
  * added: In the [Jenkins monitoring plugin](https://wiki.jenkins-ci.org/display/JENKINS/Monitoring), display of **build steps** and their durations, in the detail of each build statistics in the "/monitoring/nodes" page. See [Example](https://wiki.jenkins-ci.org/download/attachments/41877877/example_buildsteps.png)

### 1.51.0 ###

  * fix: when using java 8, cpu graph was not displayed
  * fix [issue 396](https://code.google.com/p/javamelody/issues/detail?id=396): Call to TraversableResolver.isReachable() throws exception "Can't find a delegate"
  * fix [issue 399](https://code.google.com/p/javamelody/issues/detail?id=399): Report of large Nb of used jdbc connections with Spring Routing Datasource
  * fix "ORA-00911: invalid character", when using the sql-transform-pattern parameter, for the display of the explain plan of some sql requests (Oracle database only, [revision 3799](https://code.google.com/p/javamelody/source/detail?r=3799))
  * Drop Java 5 support. Said otherwise, a JDK or a JRE v1.6 or later is needed for javamelody v1.51 or later. For Java 5, previous versions of javamelody can still be used. ([revision 3795](https://code.google.com/p/javamelody/source/detail?r=3795))
  * Optimized desktop UI startup time, by downloading desktop app and caching locally ([revision 3762](https://code.google.com/p/javamelody/source/detail?r=3762))
  * improved: abstract very long sql requests ([issue 403](https://code.google.com/p/javamelody/issues/detail?id=403))
  * improved: in a graph detail, a checkbox can now hide maximum values in the graph, so that average values are better displayed when much lower than the maximum ([issue 368](https://code.google.com/p/javamelody/issues/detail?id=368))
  * added: PID in the heap dump file name ([revision 3773](https://code.google.com/p/javamelody/source/detail?r=3773))

### 1.50.0 ###

  * JavaMelody license changed from [LGPL](https://www.gnu.org/licenses/lgpl.html) to [ASL](http://www.apache.org/licenses/LICENSE-2.0), as discussed here: https://groups.google.com/forum/#!topic/javamelody/JI3yNH0Xmb0/discussion ([revision 3649](https://code.google.com/p/javamelody/source/detail?r=3649))
  * [Downloads](https://code.google.com/p/javamelody/wiki/Downloads) moved to [GitHub](https://github.com/javamelody/javamelody/releases)

  * fix [issue 370](https://code.google.com/p/javamelody/issues/detail?id=370): work around ConcurrentModificationException during Tomcat startup (which is a [Tomcat bug](https://issues.apache.org/bugzilla/show_bug.cgi?id=56082))
  * fix [issue 378](https://code.google.com/p/javamelody/issues/detail?id=378): Slf4j system, with log4j-over-slf4j, is not detected with version 1.7.6 of slf4j (thanks to _jgraglia_)
  * fix compatibility of jdbc datasource monitoring with **WildFly** ([revision 3686](https://code.google.com/p/javamelody/source/detail?r=3686))
  * fix [issue 386](https://code.google.com/p/javamelody/issues/detail?id=386): IllegalArgumentException: No enum const class ..., in Turkish
  * fix: In the [JIRA/Confluence/Bamboo plugin](https://plugins.atlassian.com/plugins/net.bull.javamelody), fix compatibility with some old Confluence versions, such as 3.5.13 ([revision 3640](https://code.google.com/p/javamelody/source/detail?r=3640))
  * fix from [grails PR 17](https://github.com/evernat/grails-melody-plugin/pull/17): In the JavaMelody Grails plugin, verifying that property exists using hasProperty (thanks to _stiancor_)
  * fix from [grails PR 16](https://github.com/evernat/grails-melody-plugin/pull/16): In the JavaMelody Grails plugin, getting the disabled property from the grails javamelody config when running test-app (thanks to _stiancor_)
  * fix: the optional collector server may stop collecting in case of errors in notifications ([revision 3703](https://code.google.com/p/javamelody/source/detail?r=3703))
  * improved: In the US, depending on the browser's language or on the javamelody parameter "locale", the paper size is now Letter in the US, instead of A4 like in the other countries. ([revision 3679](https://code.google.com/p/javamelody/source/detail?r=3679), thanks to _Dennis_)
  * improved css styles: font finally fixed to Arial/Helvetica ([revision 3718](https://code.google.com/p/javamelody/source/detail?r=3718)). Note: You can [customize styles and other resources](UserGuideAdvanced#Customizing_styles,_icons_and_other_resources_in_the_html_report.md).

  * added: **Menu**. A floating button is available on the right of the main report to drag a menu in or out. The menu displays the list of chapters in the report and allows to jump easily between them, see [Demo](Demo.md) ([revision 3705](https://code.google.com/p/javamelody/source/detail?r=3705)).
  * added: **Custom reports**. You can now include links to your custom reports in the floating menu described above.
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
  * added: **JPA monitoring**, inspired by [Apache Sirona](http://sirona.incubator.apache.org/)

If you use JPA with a persistence.xml file, you can now monitor main calls on the JPA [EntityManager](http://docs.oracle.com/javaee/7/api/javax/persistence/EntityManager.html) (_since v1.50_).

To enable the JPA monitoring with JPA statistics in the report, set `net.bull.javamelody.JpaPersistence` as JPA Provider in your persistence.xml file. For example:
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
But if that's not the case of if you want to force the implementation set the property `net.bull.javamelody.jpa.provider` to the real implementation you want. For example:
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


### 1.49.0 ###

  * fix [issue 367](https://code.google.com/p/javamelody/issues/detail?id=367): soap error, in java 1.5
  * fix ~~[liferay issue 2](https://github.com/evernat/liferay-javamelody/issues/2)~~: The [Liferay plugin](UserGuide#Liferay_Plugin.md) v1.48.0 does not work with Liferay 6.2.0 GA1 CE
  * fix [issue 361](https://code.google.com/p/javamelody/issues/detail?id=361): InvocationTargetException when Accessing Monitoring URL on JIRA, with v1.48.0
  * improved: In the [JIRA/Confluence/Bamboo plugin](https://plugins.atlassian.com/plugins/net.bull.javamelody), display the usernames in the list of http sessions ([revision 3590](https://code.google.com/p/javamelody/source/detail?r=3590))
  * improved: When I call the "Invalidate http sessions" action, invalidate all sessions except mine (I can still invalidate my session individually after that, [revision 3591](https://code.google.com/p/javamelody/source/detail?r=3591))
  * improved: In the list of http sessions, a bullet shows which one is my own session, if I have a session ([revision 3603](https://code.google.com/p/javamelody/source/detail?r=3603))
  * added: In the [Jenkins monitoring plugin](https://wiki.jenkins-ci.org/display/JENKINS/Monitoring), for each individual node (each slave in http://yourhost/computer), reports and actions are available from the "Monitoring" page in the contextual menu or in the detail of the node:
    * Threads, process list, MBeans of that node only
    * Heap histogram of that node
    * Actions for GC, heap dump
    * (~~[JENKINS-20935](https://issues.jenkins-ci.org/browse/JENKINS-20935)~~, with help from _Oleg Nenashev_)
  * improved css styles with the come back of shadows in Firefox and with hovers for images, see [Demo](Demo.md) ([revision 3614](https://code.google.com/p/javamelody/source/detail?r=3614)). Note: You can [customize styles and other resources](UserGuideAdvanced#Customizing_styles,_icons_and_other_resources_in_the_html_report.md).

### 1.48.0 ###

  * fix issue ~~[JENKINS-20352](https://issues.jenkins-ci.org/browse/JENKINS-20532)~~: In the [Jenkins monitoring plugin](https://wiki.jenkins-ci.org/display/JENKINS/Monitoring), HTTP session count is high, since Jenkins v1.535 ([revision 3569](https://code.google.com/p/javamelody/source/detail?r=3569))
  * fix [issue 359](https://code.google.com/p/javamelody/issues/detail?id=359): Support Confluence 5.3
  * fix [issue 354](https://code.google.com/p/javamelody/issues/detail?id=354): With the java update 1.7\_u45, the **Desktop app** of the monitoring does not start
  * fix [issue 355](https://code.google.com/p/javamelody/issues/detail?id=355): ClassNotFoundException when serializing session breaks the sessions view
  * fix [issue 350](https://code.google.com/p/javamelody/issues/detail?id=350): Hotspots are not being gathered when using centralized monitoring (with help from _kalnas2_)
  * fix: In the [Liferay plugin](UserGuide#Liferay_Plugin.md), to secure the access to the monitoring page, authentication and portal "Administrator" role is now required.
  * [Source](https://github.com/evernat/liferay-javamelody), [downloads](https://github.com/evernat/liferay-javamelody/releases) and [issues](https://github.com/evernat/liferay-javamelody/issues?page=1&state=open) of the [Liferay plugin](UserGuide#Liferay_Plugin.md) are moved to GitHub. You can submit [pull requests](https://github.com/evernat/liferay-javamelody/pulls) to fix issues.
  * Optimize jdbc overhead, when there are more than 100000 sql hits per minute ([issue 353](https://code.google.com/p/javamelody/issues/detail?id=353), with help from _kalnas2_)
  * New parameter to optimize opened jdbc connections, when there are more than 100000 sql hits per minute ([issue 352](https://code.google.com/p/javamelody/issues/detail?id=352))
  * display incoming **web-services SOAP** methods in the http statistics ([issue 228](https://code.google.com/p/javamelody/issues/detail?id=228), thanks to _roy.paterson_)

### 1.47.0 ###

  * fix [issue 339](https://code.google.com/p/javamelody/issues/detail?id=339): "One day" memory leak
  * fix [issue 346](https://code.google.com/p/javamelody/issues/detail?id=346): XSS through X-Forwarded-For header spoofing
  * fix [issue 328](https://code.google.com/p/javamelody/issues/detail?id=328): NoSuchFieldError: readOnly when using v1.46 with jrobin 1.5.14 (instead of jrobin 1.5.9 as documented)
  * fix [issue 335](https://code.google.com/p/javamelody/issues/detail?id=335): centralized server not compatible with ehcache 2.7.2
  * fix [issue 232](https://code.google.com/p/javamelody/issues/detail?id=232): Use the path of -XX:HeapDumpPath=/tmp if defined, for the directory of heap dump files (otherwise use the temp directory of the server as before)
  * fix ~~[grails issue 3](https://github.com/evernat/grails-melody-plugin/issues/3)~~: In the [JavaMelody Grails plugin](http://www.grails.org/plugin/grails-melody), JDBC DataSource is not wrapped, SQL requests are not monitored (fixed in version 1.47.2 of the plugin, thanks to _sergiomichels_)
  * fix ~~[grails issue 11](https://github.com/evernat/grails-melody-plugin/issues/11)~~: In the JavaMelody Grails plugin, private def closure causes MissingMethodException (thanks to _Yunpeng Bai_)
  * fix ~~[grails issue 13](https://github.com/evernat/grails-melody-plugin/issues/13)~~: In the JavaMelody Grails plugin, even if the plugin is disabled via config option, Service Proxys are still attached (thanks to _Yunpeng Bai_)
  * fix ~~[grails issue 9](https://github.com/evernat/grails-melody-plugin/issues/9)~~: In the JavaMelody Grails plugin, Overloaded service method does not work (thanks to _macrosak_)
  * added: In the optional collect server, send an email when an application becomes unavailable. For this, it will be needed to configure a mail session and 2 javamelody parameters like for [the reports by mail](UserGuide#14._Weekly,_daily_or_monthly_reports_by_mail.md) (for example, in the Tomcat context of the collector webapp. [Issue 333](https://code.google.com/p/javamelody/issues/detail?id=333))
  * added: [MonitoringTargetInterceptor](https://code.google.com/p/javamelody/source/browse/trunk/javamelody-core/src/main/java/net/bull/javamelody/MonitoringTargetInterceptor.java) as an alternative to [MonitoringInterceptor](https://code.google.com/p/javamelody/source/browse/trunk/javamelody-core/src/main/java/net/bull/javamelody/MonitoringInterceptor.java) for EJB monitoring
  * added: **detect cpu hotspots** ([issue 149](https://code.google.com/p/javamelody/issues/detail?id=149), with some ideas from _Cédric Lime_).

In the monitoring reports, the new **Hotspots** screen displays CPU hotspots in executed methods for all the JVM. Like javamelody, the overhead of hotspots is low: it is based on sampling of stack-traces of threads, without instrumentation. And like javamelody, it is made to be always active if enabled. (It is currently not enabled by default. It may be enabled by default in the future.) Two parameters can be defined in web.xml, in context or in system properties like the other [javamelody parameters](UserGuide#6._Optional_parameters.md):

  * "sampling-seconds" to enable the sampling and to define its period. A period of 10 seconds can be recommended to have the lowest overhead, but then a few hours may be needed to have significant results for a webapp under real use. If you don't mind having a bit more overhead or want a result faster in test, you can use a value of 1 or 0.1 in second for this parameter.
  * "sampling-excluded-packages" to change the list of the excluded packages ("java,sun,com.sun,javax,org.apache,org.hibernate,oracle,org.postgresql,org.eclipse" by default)

### 1.46.0 ###

  * fix [issue 321](https://code.google.com/p/javamelody/issues/detail?id=321): javamelody disables Tomcat 7 distributed sessions when using SimpleTcpCluster
  * fix [issue 96](https://code.google.com/p/javamelody/issues/detail?id=96): fix redeployment in tomcat when javamelody and jrobin jar files are in tomcat/lib and fix redeployment in WebLogic Server
  * fix [issue 313](https://code.google.com/p/javamelody/issues/detail?id=313): NoClassDefFoundError when IBM's JVM is used
  * fix: used jdbc connections count is sometimes incorrect ([revision 3437](https://code.google.com/p/javamelody/source/detail?r=3437))
  * In Jenkins, use a timeout for the monitoring of slaves, when a slave is online but does not respond
  * In Jenkins, fix NPE when manually purging the obsolete monitoring files of slaves
  * New [javamelody plugin](https://github.com/evernat/sonar-javamelody) to monitor a [Sonar](http://www.sonarqube.org/) server. Documentation is [here](UserGuide#Sonar_Plugin.md)

### 1.45.2 (JIRA / Confluence / Bamboo plugin) ###

  * fix [issue 317](https://code.google.com/p/javamelody/issues/detail?id=317): compatibility of the plugin with JIRA 5.1 and 5.2

### 1.45.1 (JIRA / Confluence / Bamboo plugin) ###

  * fix [issue 314](https://code.google.com/p/javamelody/issues/detail?id=314): compatibility of the plugin with JIRA 6

### 1.45.0 ###

  * [JavaMelody Grails plugin](http://www.grails.org/plugin/grails-melody) is published again at grails.org. You can install the plugin in your grails app with just one command:

> `grails install-plugin grails-melody`

  * [Source](https://github.com/evernat/grails-melody-plugin) and [issues](https://github.com/evernat/grails-melody-plugin/issues?page=1&state=open) of the [JavaMelody Grails plugin](http://www.grails.org/plugin/grails-melody) are moved to GitHub. You can submit [pull requests](https://github.com/evernat/grails-melody-plugin/pulls) to fix issues.
  * known incompatibility of the JIRA plugin with JIRA 6.0, [issue 314](https://code.google.com/p/javamelody/issues/detail?id=314)
  * fix: In the Liferay plugin, fix SAXReader "Connection timed out" exception, when http.proxyHost is not specified ([revision 3347](https://code.google.com/p/javamelody/source/detail?r=3347))
  * fix [issue 298](https://code.google.com/p/javamelody/issues/detail?id=298): for the database reports, spclocation was removed in postgresql 9.2 and datconfig was removed in postgresql 9.0
  * added: For Postgresql, enhance the locks report by displaying the relation name and the database name of each lock ([revision 3403](https://code.google.com/p/javamelody/source/detail?r=3403))
  * fix [issue 303](https://code.google.com/p/javamelody/issues/detail?id=303): SQL monitoring doesn't work on SpringSource tc Runtime
  * fix: adding an application in the collect server works, then does not work anymore after restart of the collect server, if the access to the application data is secured by basic authentication with the [documented](UserGuideAdvanced#5._Security_with_a_collect_server.md) login "monitoring" ([revision 3355](https://code.google.com/p/javamelody/source/detail?r=3355))
  * added: button to kill a thread from the current requests ([issue 302](https://code.google.com/p/javamelody/issues/detail?id=302))
  * added: button to donate in the html reports, to better inform users of this possibility

### 1.44.0 ###

  * fix: In Jenkins, it is useless to display the sql hits for the current requests
  * fix: In Jenkins, better aggregation of http requests in the statistics, for URLs /adjuncts/... and /$stapler/bound/... (also reduces the disk space used to store data)
  * fix: When opening the detail of a Quartz job, display the statistics of the job, including sql or services called. And when looking for the usages of a sql or service, include the results for jobs.
  * fix [issue 289](https://code.google.com/p/javamelody/issues/detail?id=289): JBoss AS 7 configuration should not be needed
  * fix [issue 293](https://code.google.com/p/javamelody/issues/detail?id=293): Ehcache 2.7.0 doesn't work with javamelody. Note that because of an [ehcache bug](https://jira.terracotta.org/jira/browse/EHC-1010), the value of the cache efficiency (hits/access) displayed by javamelody is incorrect when using ehcache 2.7.0.
  * added: because getting the debugging logs in log files may be difficult, display the debugging logs such as javamelody initialization, at the bottom of the html report
  * added: display of values in the JNDI tree ([issue 287](https://code.google.com/p/javamelody/issues/detail?id=287), thanks to _derjust_)
  * doc added: For Jenkins, [Monitoring scripts](https://wiki.jenkins-ci.org/display/JENKINS/Monitoring+Scripts) can be executed using the [Jenkins Script Console](https://wiki.jenkins-ci.org/display/JENKINS/Jenkins+Script+Console).

### 1.43.0 ###

  * fix [issue 274](https://code.google.com/p/javamelody/issues/detail?id=274): Monitoring of SQL requests after hot redeploy in JBoss, Glassfish or WebLogic (thanks to _lastmike_)
  * fix [issue 281](https://code.google.com/p/javamelody/issues/detail?id=281): Database not recognised in Websphere Application Server (thanks to _lastmike_)
  * fix [issue 269](https://code.google.com/p/javamelody/issues/detail?id=269): Compatibility of the JIRA/Confluence/Bamboo plugin with some other plugins
  * fix [issue 270](https://code.google.com/p/javamelody/issues/detail?id=270): French Canadians can't choose a customized period
  * fix: in the current requests, use the chosen period to display the statistics of mean times, besides the elapsed times
  * optimization of SpringDataSourceBeanPostProcessor ([issue 282](https://code.google.com/p/javamelody/issues/detail?id=282), thanks to _lastmike_)
  * added: PDF report of the current requests
  * added: Support for Sybase in the database reports ([issue 276](https://code.google.com/p/javamelody/issues/detail?id=276), thanks to _lastmike_)
  * added: javamelody parameter "locale" to fix the locale of the reports, whatever the language in the browser ([issue 271](https://code.google.com/p/javamelody/issues/detail?id=271), thanks to _xiukongtiao_)
  * added: possibility to clear a cache individually ([issue 283](https://code.google.com/p/javamelody/issues/detail?id=283), thanks to _idrahou_)

### 1.42.0 ###

  * fix [issue 242](https://code.google.com/p/javamelody/issues/detail?id=242): Just after a restart, the statistics of services (spring, ejb, guice) may be incorrectly summed with past values, if they are initialized AFTER the MonitoringFilter
  * fix [issue 261](https://code.google.com/p/javamelody/issues/detail?id=261): Use the thread context-classloader to load the jdbc driver if used directly
  * fix [issue 262](https://code.google.com/p/javamelody/issues/detail?id=262): NullPointerException in MonitoringFilter.doFilter(), when the webapp is undeployed and after a timeout
  * fix [issue 267](https://code.google.com/p/javamelody/issues/detail?id=267): Suppress "IllegalArgumentException: Database type unknown: jdbc:sqlserver"
  * fix [issue 258](https://code.google.com/p/javamelody/issues/detail?id=258): The IText files causes NullPointerException in FlyingSaucer
  * fix: in the optional collect server, use parallel collect threads for the case when there are some slow applications to monitor (from the [users' group](https://groups.google.com/d/msg/javamelody/osIyFXlEYTM/PhantDkWx-gJ))
  * added: PDF report for the JNDI tree. [Example](http://demo.javamelody.cloudbees.net/monitoring?part=jndi&path=comp&format=pdf) in the demo.
  * added: plugin to monitor **Liferay v6.1** or later. Documentation is [here](UserGuide#Liferay_Plugin.md)

  * added (beta): New alternative User Interface for the monitoring reports with a **Rich Desktop Application**. Highlights :
    * All reports and data like in the web UI
    * Exports to PDF, XML and JSON formats, even if the monitored application does not have the dependencies to do the same in the web UI
    * Tabs to view different reports
    * Columns of tables may be sorted, resized and moved
    * Exports with right-click for all tabular data to CSV, PDF, RTF, HTML, XML, JSON formats
    * Shortcuts, such as F5
    * Started in one-click with the "Desktop" link at the top of the web UI. The required libraries are downloaded automatically from googlecode on the first launch (_this could work better_).
    * Uses JavaWebStart and requires [JRE 1.7](http://java.com) on the client
    * This new UI may be an alternative to the web UI for advanced users or for exports

### 1.41.0 ###

  * [issue 244](https://code.google.com/p/javamelody/issues/detail?id=244) and [issue 245](https://code.google.com/p/javamelody/issues/detail?id=245) were Resin issues when using javamelody and they are fixed in [Resin v4.0.31](http://caucho.com/resin-4.0/changes/changes.xtp#4031Sep122012)
  * fix [issue 252](https://code.google.com/p/javamelody/issues/detail?id=252): Add XSS protection
  * fix [issue 255](https://code.google.com/p/javamelody/issues/detail?id=255): exception while collecting data java.io.FileNotFoundException: Could not open ....rrd [existent](non.md)
  * fix [issue 248](https://code.google.com/p/javamelody/issues/detail?id=248): JsfActionListener sends two requests when javamelody is disabled
  * enhanced the display of the list of applications in the optional collector server, when there are more than 10 applications ([revision 3047](https://code.google.com/p/javamelody/source/detail?r=3047))
  * added: allow extend servlet security check ([issue 256](https://code.google.com/p/javamelody/issues/detail?id=256))

### 1.40.0 ###

  * Upgrade xstream to 1.4.2 in the optional collect server, and in the [JIRA/Confluence/Bamboo plugin](https://plugins.atlassian.com/plugins/net.bull.javamelody).
  * fix [issue 231](https://code.google.com/p/javamelody/issues/detail?id=231): In [JIRA/Confluence/Bamboo plugin](https://plugins.atlassian.com/plugins/net.bull.javamelody), incompatibility with Bamboo remote agents.
  * fix [issue 233](https://code.google.com/p/javamelody/issues/detail?id=233): The monitoring reports should not set autoCommit(false) on JDBC connections obtained from datasources.
  * fix [issue 243](https://code.google.com/p/javamelody/issues/detail?id=243): Spring config files may trigger internet access
  * fix [Jenkins issue 14050](https://issues.jenkins-ci.org/browse/JENKINS-14050) also for the /nodes reports in the [Jenkins monitoring plugin](https://wiki.jenkins-ci.org/display/JENKINS/Monitoring).
  * added in the external API: XML and JSON exports of MBeans, and of JNDI tree given a JNDI context path. Documentation is [here](ExternalAPI#XML.md). For example: [MBeans](http://demo.javamelody.cloudbees.net/monitoring?format=xml&part=mbeans) and [JNDI](http://demo.javamelody.cloudbees.net/monitoring?format=json&part=jndi&path=comp).
  * added: The new parameter `gzip-compression-disabled` makes possible to disable the compression of the monitoring reports, for example if there is another mechanism which would compress a second time (false by default: the reports are compressed by default if the browser supports it). Documented with the other optional parameters in the [user guide](UserGuide#6._Optional_parameters.md).

### 1.39.0 ###

  * fix [Jenkins issue 14050](https://issues.jenkins-ci.org/browse/JENKINS-14050), and also when security is enabled in Jenkins: Unreadable HTML response for the monitoring reports in the [Jenkins monitoring plugin](https://wiki.jenkins-ci.org/display/JENKINS/Monitoring). Compression of the monitoring reports is now disabled in the plugin and the reports will be compressed by Jenkins starting with v1.470.
  * fix [issue 230](https://code.google.com/p/javamelody/issues/detail?id=230): NoClassDefFoundError: javax/servlet/AsyncContext in Tomcat 5.5

### 1.38.0 ###

  * fix [Jenkins issue 14050](https://issues.jenkins-ci.org/browse/JENKINS-14050), but only when security is not enabled in Jenkins: Unreadable HTML response for the monitoring reports in the [Jenkins monitoring plugin](https://wiki.jenkins-ci.org/display/JENKINS/Monitoring)
  * fix major [issue 138](https://code.google.com/p/javamelody/issues/detail?id=138): It is now possible to monitor SQL requests and jdbc connections with a JNDI DataSource in GlassFish v3, (thanks to _Pieter_ for the analysis and to _Bhun Kho_ for the initial workaround). This can work with whatever version of JavaMelody from 1.23 to the latest. Documentation is [here](UserGuideAdvanced#Monitoring_of_sql_requests_and_of_jdbc_connections_in_v3+.md)
  * fix [issue 229](https://code.google.com/p/javamelody/issues/detail?id=229): In Glassfish v2.1.1, Exception in NamingManagerImpl copyMutableObject() for a JNDI DataSource
  * fix [issue 217](https://code.google.com/p/javamelody/issues/detail?id=217): AsyncContext.getResponse().getWriter() throws IllegalStateException "getOutputStream() has already been called for this response" in Tomcat
  * fix [issue 198](https://code.google.com/p/javamelody/issues/detail?id=198): In [JIRA/Confluence/Bamboo plugin](https://plugins.atlassian.com/plugins/net.bull.javamelody), monitoring of http sessions was not working after a few minutes or a few hours with JIRA 4.4 or Confluence 4.1
  * fix minor [issue 210](https://code.google.com/p/javamelody/issues/detail?id=210): In [JIRA/Confluence/Bamboo plugin](https://plugins.atlassian.com/plugins/net.bull.javamelody) with a JIRA server, there was sometimes an error in log about "jira-web-item".
  * In JIRA 5.0, changing the dbconfig.xml with a JdbcDriver does not work anymore to monitor jdbc connections and SQL requests: the [user guide](UserGuide#Atlassian_JIRA,_Confluence_and_Bamboo_Plugin.md) has been updated to use a datasource from jndi instead ([issue 225](https://code.google.com/p/javamelody/issues/detail?id=225)).
  * added: When using the [optional collect server](UserGuideAdvanced#Optional_centralization_server_setup.md), enable use of http Basic authentication to secure access to the monitored webapp, for example when the ip-address of the collect server is not known upfront ([issue 214](https://code.google.com/p/javamelody/issues/detail?id=214), thanks to _purnhar_). Documentation is [here](UserGuideAdvanced#5._Security_with_a_collect_server.md).
  * added: When using the [optional collect server](UserGuideAdvanced#Optional_centralization_server_setup.md), the size of the stream, between the monitored webapp and the collect server, is now written in the log of the collect server for each call. (Note that the stream is compressed if more than 50 KB ; when compressed, the log gives the compressed size. [revision 2828](https://code.google.com/p/javamelody/source/detail?r=2828))

### 1.37.0 ###

  * fix [issue 207](https://code.google.com/p/javamelody/issues/detail?id=207): Incompatibility with servlet api 2.4 (Tomcat 5.5) in javamelody 1.36.0
  * fix [issue 205](https://code.google.com/p/javamelody/issues/detail?id=205) and [issue 206](https://code.google.com/p/javamelody/issues/detail?id=206): delegate the JSF ActionListener to the next JSF ActionListener if one was defined in the faces-config.xml file (thanks to _Patrick Dobler_)
  * fix: the SpringDataSourceBeanPostProcessor now implements PriorityOrdered to be initialized sooner ([revision 2724](https://code.google.com/p/javamelody/source/detail?r=2724))
  * fix [issue 187](https://code.google.com/p/javamelody/issues/detail?id=187): can't protect Melody grails plugin with spring security core (thanks to _Burt Beckwith_)
  * fix ~~[GPMELODY-4](http://jira.grails.org/browse/GPMELODY-4)~~: Load the Melody grails plugin after shiro (thanks to _Tomas Lin_).
  * added: Display the properties of the new [Tomcat jdbc](http://tomcat.apache.org/tomcat-7.0-doc/jdbc-pool.html) datasources in System informations like for Tomcat DBCP, in particular display the max number of active connections next to the current number of active connections ([issue 29](https://code.google.com/p/javamelody/issues/detail?id=29)).
  * added: Reduce the possibility of storage overload by deleting automatically the obsolete RRD files (which were not updated for the last 3 months), and which were for requests which do not exist anymore ([revision 2750](https://code.google.com/p/javamelody/source/detail?r=2750) and [r2753](https://code.google.com/p/javamelody/source/detail?r=2753)). Note: the size of existing RRD files is fixed for ever and old .ser.gz files are already automatically deleted after a year.
  * added: Display the disk usage of the storage at the bottom of the report ([revision 2761](https://code.google.com/p/javamelody/source/detail?r=2761))

### 1.36.0 ###

<a href='http://www.cloudbees.com/'><img src='http://javamelody.googlecode.com/svn/trunk/javamelody-core/src/site/resources/images/Button-Built-on-CB-1.png' alt='Built on CloudBees' width='20%' /></a>Thanks to [CloudBees](http://www.cloudbees.com), there is now a public [Jenkins](http://jenkins-ci.org) **Continuous Integration** server for the JavaMelody project:

> Anytime, you can download the latest **[nightly build](https://javamelody.ci.cloudbees.com/job/javamelody/)** (beta, snapshot from trunk). You can also browse the latest [javadoc](https://javamelody.ci.cloudbees.com/job/javamelody/site/apidocs/index.html), [sources](https://javamelody.ci.cloudbees.com/job/javamelody/site/xref/index.html) and other [reports](https://javamelody.ci.cloudbees.com/job/javamelody/). There is also a [continuous integration](https://javamelody.ci.cloudbees.com/job/jenkins%20plugin/) for the [Jenkins Monitoring plugin](https://wiki.jenkins-ci.org/display/JENKINS/Monitoring).

> And there is a public **demo of JavaMelody**. It is based on the latest source in trunk and is continuously deployed with DEV@CLOUD. It features http (GWT), services (java interfaces) and SQL (jdbc, Mysql) monitoring in particular.
    * First, you can play with this quick and dirty [GWT application](http://demo.javamelody.cloudbees.net/) to have some data (thanks Benjamin)
    * Then you can view the charts and the statistics in the [monitoring reports](http://demo.javamelody.cloudbees.net/monitoring) (no password)
    * Some JavaMelody features are not in the demo, see the [Screenshots](Screenshots.md) for other features.

> Other notes:
    * fix blocking [issue 194](https://code.google.com/p/javamelody/issues/detail?id=194) and [issue 143](https://code.google.com/p/javamelody/issues/detail?id=143), [issue 199](https://code.google.com/p/javamelody/issues/detail?id=199), [issue 200](https://code.google.com/p/javamelody/issues/detail?id=200): The jrobin v1.5.9 artifacts are now available in the **Maven central** repository as you can see [here](http://search.maven.org/#artifactdetails|org.jrobin|jrobin|1.5.9|jar). The external (and currently unreachable) repository in the pom is now useless and removed.
    * fix minor [issue 192](https://code.google.com/p/javamelody/issues/detail?id=192): In a Bamboo server v3.1+, internal server error when viewed by non-admin user.
    * fix [issue 193](https://code.google.com/p/javamelody/issues/detail?id=193): Make "Deployment on Tomcat without modification of monitored webapps" work like before v1.32
    * fix [issue 189](https://code.google.com/p/javamelody/issues/detail?id=189): Should not set autoCommit to false on datasource connections
    * fix [issue 195](https://code.google.com/p/javamelody/issues/detail?id=195): In the database reports, compatibility of "innodb status" with both Mysql 5.5 and Mysql 4
    * fix [issue 201](https://code.google.com/p/javamelody/issues/detail?id=201): Unable to render MBeans tree when some JMX-retrieval exception occurs
    * fix [issue 204](https://code.google.com/p/javamelody/issues/detail?id=204): There should not be any blocking exception when initializing the JSF action listener
    * added: monitoring-spring-datasource.xml, as an alternative to monitoring-spring.xml, to monitor only the datasources when there is some conflict ([issue 172](https://code.google.com/p/javamelody/issues/detail?id=172)). See [doc](UserGuide#7._JDBC.md) at the end of the JDBC chapter.
    * added: Possibility to customize styles, icons and other resources in the html reports ([issue 196](https://code.google.com/p/javamelody/issues/detail?id=196)). See [doc](UserGuideAdvanced#Customizing_styles,_icons_and_other_resources_in_the_html_report.md) in the advanced user guide. Please submit your best [CSS file](http://code.google.com/p/javamelody/source/browse/trunk/javamelody-core/src/main/resources/net/bull/javamelody/resource/monitoring.css) to the [users' group](http://groups.google.com/group/javamelody).
    * added: if there are deadlocked threads in the JVM, print a warning message with the names of the deadlocked threads at the top of threads dump, like in the html and pdf reports ([revision 2678](https://code.google.com/p/javamelody/source/detail?r=2678))

### 1.35.0 ###

  * fix [issue 185](https://code.google.com/p/javamelody/issues/detail?id=185): In [JIRA/Confluence/Bamboo plugin](https://plugins.atlassian.com/plugins/net.bull.javamelody), fix the compatibility of checking the admin permission with Confluence 4.1.4 and after.
  * fix: In the plugin managers of JIRA/Confluence/Bamboo, display the correct version of the plugin ([revision 2530](https://code.google.com/p/javamelody/source/detail?r=2530))
  * fix [issue 178](https://code.google.com/p/javamelody/issues/detail?id=178): In the [Jenkins/Hudson plugin](https://wiki.jenkins-ci.org/display/JENKINS/Monitoring) and in [JIRA/Confluence/Bamboo plugin](https://plugins.atlassian.com/plugins/net.bull.javamelody), since v1.32, "Nb of http sessions" is 0 and "View http sessions" is always empty.
  * fix [JNDI error](https://groups.google.com/group/javamelody/browse_thread/thread/aa7198fab0ccbd51) in the vFabric server ([revision 2540](https://code.google.com/p/javamelody/source/detail?r=2540))
  * added: In the [Jenkins/Hudson plugin](https://wiki.jenkins-ci.org/display/JENKINS/Monitoring), new graphic of "Build queue length" in the monitoring of nodes, next to the graphic of "Running builds" ([revision 2515](https://code.google.com/p/javamelody/source/detail?r=2515), done in Jenkins Hackathon, with help from Kohsuke Kawaguchi)
  * added: some "database reports" for **DB2**, in the "System information" part: MON\_CURRENT\_SQL, MON\_DB\_SUMMARY, MON\_LOCKWAITS, MON\_SERVICE\_SUBCLASS\_SUMMARY, MON\_CURRENT\_UOW, MON\_WORKLOAD\_SUMMARY, MON\_GET\_CONNECTION and fix of SNAPSHOT\_DYN\_SQL ([issue 179](https://code.google.com/p/javamelody/issues/detail?id=179), thanks to _Peter James and his DBA from humanservices.gov.au_)
  * added: optional parameter (dns-lookups-disabled) to allow disabling of dns lookups with InetAddress.getLocalHost() to prevent long hangs on startup/shutdown in some particular environments ([issue 181](https://code.google.com/p/javamelody/issues/detail?id=181), thanks to _r6squeegee_)
  * added: In the [Grails Melody plugin](http://www.grails.org/plugin/grails-melody), read the configuration parameters from both Config.groovy and GrailsMelodyConfig.groovy ([issue 180](https://code.google.com/p/javamelody/issues/detail?id=180), thanks to _padcom_)
  * added: possibility to customize "name" in the statistics (long standing [issue 31](https://code.google.com/p/javamelody/issues/detail?id=31)). Note that instance creation (such as String concatenation) must be reduced to the minimum, otherwise there will be a performance overhead. It is really not recommended to customize.

### 1.34.0 ###

  * fix [issue 121](https://code.google.com/p/javamelody/issues/detail?id=121) and [issue 175](https://code.google.com/p/javamelody/issues/detail?id=175): Incompatibility with **Quartz 2** (thanks to _rogerc at costumercentrix.com_)
  * fix [issue 164](https://code.google.com/p/javamelody/issues/detail?id=164): **JBoss 7** support for SQL monitoring with JNDI datasource (thanks to _sarnowski at new-thoughts.org_)
  * fix [issue 166](https://code.google.com/p/javamelody/issues/detail?id=166): jboss-as-client and javamelody log4j conflict (java.lang.IncompatibleClassChangeError)
  * fix [issue 170](https://code.google.com/p/javamelody/issues/detail?id=170): Plugin for JIRA / Confluence / Bamboo: Compatibility with JIRA 5.0 RC
  * fix: if the http request contains for example ";jsessionid=1234567890ABCDEF", when the browser does not accept cookies, then that part is automatically ignored for the statistics ([revision 2363](https://code.google.com/p/javamelody/source/detail?r=2363))
  * fix fonts in the PDF reports for Chinese people, when the first PDF report was made for non Chinese people ([revision 2441](https://code.google.com/p/javamelody/source/detail?r=2441))
  * added: graphic over time of the "**Transactions per minute**" in "Other charts" of the reports. This graphic is similar to "Http hits per minute" and to "Sql hits per minute", but for database(s) transactions (more precisely, for the number of opened jdbc connections per minute). The values in this graphic are typically about one tenth of the values in "Sql hits per minute", depending on the mean number of sql requests per transaction in the monitored application ([revision 2467](https://code.google.com/p/javamelody/source/detail?r=2467)).

### Maven central repository ###

**The latest release of javamelody artifacts are now available in the Maven central repository**: [javamelody-core in central](http://search.maven.org/#search|ga|1|a%3A%22javamelody-core%22) . And so, you don't need anymore to use in your pom.xml or settings.xml the http://maven.glassfish.org repository for javamelody.

(In fact, it seems that some obsolete artefacts of javamelody were recently published by Sonatype in Maven central from an old java.net repository. Seeing that, I published the latest javamelody artefacts in Maven central.)

### 1.33.0 ###

  * fix [issue 146](https://code.google.com/p/javamelody/issues/detail?id=146) and [issue 153](https://code.google.com/p/javamelody/issues/detail?id=153): Incorrect Struts statistics when two actions have the same name but are in different packages. Note: the consequence is that the names of all the Struts actions are changed in the statistics.
  * fix [issue 152](https://code.google.com/p/javamelody/issues/detail?id=152): Error with execution plan for "ALTER SESSION..."
  * fix [issue 157](https://code.google.com/p/javamelody/issues/detail?id=157): In the Grails plugin, do not add groovy meta programming on services if javamelody is disabled. ([rev 76158](http://svn.codehaus.org/grails-plugins/grails-grails-melody/trunk/GrailsMelodyGrailsPlugin.groovy))
  * fix ~~[Jenkins issue  11293](https://issues.jenkins-ci.org/browse/JENKINS-11293)~~: Monitoring plugin not installed because of NoClassDefFoundError: org.slf4j.ILoggerFactory on IBM J9 JVM ([rev 40073](https://svn.jenkins-ci.org/trunk/hudson/plugins/monitoring/pom.xml))
  * fix: When using the collect server and if Oracle database, display the execution plan in the detail of a sql request like when the collect server is not used ([revision 2305](https://code.google.com/p/javamelody/source/detail?r=2305)).
  * fix [issue 160](https://code.google.com/p/javamelody/issues/detail?id=160): Disabling sql statistics with the "displayed-counters" parameter should not disable the connection monitoring
  * added for [issue 158](https://code.google.com/p/javamelody/issues/detail?id=158): note in the UI and in the online help to say that since ehcache v2.1, the cache statistics need to be enabled in order to display efficiency values
  * added: **JSF actions** statistics like for Struts 2 actions. If the application to monitor contains some JSF actions based on Mojarra (JSF Reference Implementation), a counter is automatically created for statistics of execution of actions. A JSF ActionListener is automatically added for the JSF application and you don't need to do anything to have JSF statistics.

### 1.32.1 ###
  * fix [issue 151](https://code.google.com/p/javamelody/issues/detail?id=151): a Java 1.6 dependency was introduced in 1.32.0 (NoSuchFieldError: ROOT)

### 1.32.0 ###
  * **Major change: System actions are now enabled by default**, including in the centralized collect server, because they are useful. If you want to disable them, as it was before by default, then you can add a system-actions-enabled parameter with value false: see the user guide or add a system property -Djavamelody.system-actions-enabled=false ([revision 2202](https://code.google.com/p/javamelody/source/detail?r=2202)).
  * fix [issue 137](https://code.google.com/p/javamelody/issues/detail?id=137): MonitoringInterceptor when Stateful EJB, Java EE 6
  * fix [issue 141](https://code.google.com/p/javamelody/issues/detail?id=141): exception while collecting data java.io.IOException: Read failed, file xyz not mapped for I/O (after out of disk space)
  * fix [issue 147](https://code.google.com/p/javamelody/issues/detail?id=147): When the monitoring filter is incorrectly defined in web.xml for recent appserver, parameters inside filter are sometimes ignored
  * fix: In the collector server, refuse to add twice one instance of an application (for example, individually and then in a cluster), because it would not work ([revision 2157](https://code.google.com/p/javamelody/source/detail?r=2157)).
  * added: Chinese translation, and fix display of Chinese characters in the JRobin graphics and in the PDF reports ([issue 150](https://code.google.com/p/javamelody/issues/detail?id=150))
  * added: Action "Generate a heap dump" added for IBM JDK like for Oracle JDK, patch by _David Karlsen_ in the [users' group](http://groups.google.com/group/javamelody/browse_thread/thread/49787fb45384b915) ([revision 2180](https://code.google.com/p/javamelody/source/detail?r=2180))
  * added: Add link "Dump threads as text" below the list of threads ([revision 2163](https://code.google.com/p/javamelody/source/detail?r=2163) and [revision 2196](https://code.google.com/p/javamelody/source/detail?r=2196))
  * added: Provide an url to test sending a pdf report by mail ([issue 145](https://code.google.com/p/javamelody/issues/detail?id=145))

### 1.31.0 ###
  * fix [issue 128](https://code.google.com/p/javamelody/issues/detail?id=128): Fix the shutdown process in JIRA 4.3.4 and clean it for Confluence/Bamboo/Jenkins/Hudson.
  * fix [issue 129](https://code.google.com/p/javamelody/issues/detail?id=129): ArrayIndexOutOfBoundsException in GWTRequestWrapper in a particular case.
  * fix [issue 134](https://code.google.com/p/javamelody/issues/detail?id=134): In JBoss, sending mails with weekly report should not need jboss-web.xml
  * [issue 133](https://code.google.com/p/javamelody/issues/detail?id=133): [Documentation](UserGuideAdvanced#Usage_of_in_JBoss_AS_7_%28which_uses_OSGI%29.md) added on OSGI configuration when using JBoss AS 7
  * To display the username in the list of http sessions, look at ACEGI\_SECURITY\_LAST\_USERNAME and SPRING\_SECURITY\_LAST\_USERNAME if getRemoteUser() was null (In particular, for Jenkins/Hudson. [Revision 2003](https://code.google.com/p/javamelody/source/detail?r=2003)).

  * The JRobin dependency is now available in the maven glassfish repository as JavaMelody: http://maven.glassfish.org/content/groups/public/org/jrobin/jrobin/1.5.9/. So it is not necessary to add the jrobin repository or the jrobin dependency in your pom.xml. The jrobin dependency is included transitively with javamelody-core. Reference: [UserGuide#Dependencies](UserGuide#Dependencies.md)

### 1.30.0 ###

  * fix [issue 116](https://code.google.com/p/javamelody/issues/detail?id=116): MBeans overview does not work with java 1.5
  * fix [issue 117](https://code.google.com/p/javamelody/issues/detail?id=117): Sorting of numbers is based on String comparison in German
  * fix [issue 122](https://code.google.com/p/javamelody/issues/detail?id=122): Average number of requests per minutes seems to be wrong, with a single day in a custom period
  * fix [issue 124](https://code.google.com/p/javamelody/issues/detail?id=124): Start date removed from the text at the top of the monitoring page, except if the "All" period is selected.

### 1.29.0 ###

  * fix [issue 106](https://code.google.com/p/javamelody/issues/detail?id=106): a few HTTP hits are lost in the statistics for the first hit(s) on new requests (no impact on statistics once requests are known)
  * fix [issue 104](https://code.google.com/p/javamelody/issues/detail?id=104): support SQL monitoring in Apache OpenEJB bundled as TomEE
  * fix [issue 107](https://code.google.com/p/javamelody/issues/detail?id=107): Hibernate factory doesn't work with HSQL with batching turned off
  * fix [issue 108](https://code.google.com/p/javamelody/issues/detail?id=108): `async-supported` added in the web-fragment.xml file to support asynchronous requests in Tomcat 7.0.
  * added: PDF report for the "Summary by class"

### 1.28.0 ###

  * fix [issue 99](https://code.google.com/p/javamelody/issues/detail?id=99): NullPointerException when displaying the list of process on AIX
  * fix [issue 101](https://code.google.com/p/javamelody/issues/detail?id=101): In the collector server, can't add an application when the applications.properties file does not exist
  * fix [issue 102](https://code.google.com/p/javamelody/issues/detail?id=102): In the collector server, NoClassDefFoundError when IBM's JVM is used
  * fix [issue 105](https://code.google.com/p/javamelody/issues/detail?id=105): Monitoring JNDI DataSource in Tomcat fails under host name other than "localhost"
  * added: Alternative in order to use AspectJ in Spring using [net/bull/javamelody/monitoring-spring-aspectj.xml](http://code.google.com/p/javamelody/source/browse/trunk/javamelody-core/src/main/resources/net/bull/javamelody/monitoring-spring-aspectj.xml), in case that proxies don't work ([documentation](UserGuide#9._Business_facades_(if_Spring).md), thanks to [\_Witek Wołejszo\_](http://touk.pl/blog/2011/03/07/javamelody-spring-and-aspectj/))

### 1.27.0 ###

  * fix [issue 97](https://code.google.com/p/javamelody/issues/detail?id=97): StackOverflowError in JBoss 4 when using JNDI dataSources with several monitored webapps (fix verified by _Saurabh Arora_).
  * grammar fixes in English user guides and online help by _Stéphanie Bourbon_
  * added: pdf report of "Runtime dependencies" for EJB3/Spring/Guice/Services (needs the iText jar dependency)
  * added: pdf report of MBeans (system actions must be [enabled](UserGuide#6._Optional_parameters.md))
  * added: [SpringDataSourceFactoryBean](http://code.google.com/p/javamelody/source/browse/trunk/javamelody-core/src/main/java/net/bull/javamelody/SpringDataSourceFactoryBean.java), as an alternative to excludedDatasources in [SpringDataSourceBeanPostProcessor](http://code.google.com/p/javamelody/source/browse/trunk/javamelody-core/src/main/java/net/bull/javamelody/SpringDataSourceBeanPostProcessor.java), in order to define which datasources are monitored ([issue 90](https://code.google.com/p/javamelody/issues/detail?id=90), patch by _David Karlsen_)
  * [JavaMelody Grails Plugin](http://www.grails.org/plugin/grails-melody): use dependency resolution ([issue 92](https://code.google.com/p/javamelody/issues/detail?id=92)).<br />Dependencies of the plugin are now declared in [BuildConfig.groovy](https://svn.codehaus.org/grails-plugins/grails-grails-melody/trunk/grails-app/conf/BuildConfig.groovy) instead of jar files in the plugin. As a consequence and starting from JavaMelody Grails plugin v1.2, a network connection will be needed the first time to download the dependencies with Ivy.

### 1.26.0 ###

  * added: **GWT-RPC detailed statistic collection feature**, contributed by _dhartford_ ([issue 88](https://code.google.com/p/javamelody/issues/detail?id=88)).<br />Breaks up general servlet calls (GwtServiceServlet) into the specific GWT-RPC calls for statistic collection (GwtServiceServlet.LOGIN GWT-RPC, GwtServiceServlet.Data\_retrieve\_method GWT-RPC, GwtServiceServlet.update\_data GWT-RPC, etc).<br />Only enabled when GWT-RPC is detected (and 'http'). Works with GWT 1.3 to 2.1 (to date).  Will not work with deRPC/RequestFactory transport mechanisms, only GWT-RPC specifically.
  * added: **Portuguese Brazil translation**, including the online help, thanks to _Luiz Gonzaga da Mata_ and _Renan Oliveira da Cunha_. This translation is the default for all Portuguese people. The first language in the browser settings should be Portuguese (pt) or Portuguese/Brazil (pt-br) to use it. The [online help in Portuguese Brazil](http://javamelody.googlecode.com/svn/trunk/javamelody-core/src/site/resources/Ajuda_on-line_do_monitoring.pdf) is also available.
  * fix [issue 85](https://code.google.com/p/javamelody/issues/detail?id=85): web-fragment schema location in **JBoss 6.0.0 CE**, patch contributed by _dhartford_
  * fix JNDI report in Glassfish, WebLogic, Jonas and Websphere
  * fix [issue 89](https://code.google.com/p/javamelody/issues/detail?id=89): Listing of MBeans fails in Jetty
  * added: If JBoss 5, display JBoss specific MBeans
  * Hudson/Jenkins Monitoring plugin:
    * fix [Jenkins issue 8344](http://issues.jenkins-ci.org/browse/JENKINS-8344) (ClassNotFoundException: net.bull.javamelody.SessionListener)
    * added: in the nodes report, chart of the number of the running builds by period

### 1.25.0 ###

  * fix: When the spring context is initialized before the web filter of the monitoring and when "displayed-counters" is not used, the spring monitoring was not displayed (Pete in the users group).
  * fix some issues in the monitoring of Hudson nodes when the operating systems of the nodes are heterogeneous
  * fix [issue 36](https://code.google.com/p/javamelody/issues/detail?id=36): Grails Melody plugin blocks closure (fixed by _Jean Barmash_ and _Liu Chao_)
  * fix [issue 79](https://code.google.com/p/javamelody/issues/detail?id=79): Spring security core integration for the Grails Melody plugin (fixed by _Liu Chao_)
  * fix [issue 80](https://code.google.com/p/javamelody/issues/detail?id=80): Memory histogram should be supported on Mac OS X
  * fix [issue 83](https://code.google.com/p/javamelody/issues/detail?id=83): StringIndexOutOfBoundsException for caches with # in the name of a cache
  * added: graphic of the mean age of the currently valid http sessions (in minutes), displayed in the "Other charts"
  * added: if JRockit, display the JRockit specific MBeans
  * added: pdf reports of http sessions and of heap histogram
  * This time the artifacts are published in the usual maven repository.

### 1.24.0 ###

  * fix [issue 67](https://code.google.com/p/javamelody/issues/detail?id=67): EhCache 1.4.x detection is not robust enough (NoClassDefFoundError)
  * fix [issue 68](https://code.google.com/p/javamelody/issues/detail?id=68): support more prefixes for JNDI names of datasources
  * fix [issue 69](https://code.google.com/p/javamelody/issues/detail?id=69): NullPointerException when getting resources if mime-type is null
  * fix [issue 71](https://code.google.com/p/javamelody/issues/detail?id=71): StringIndexOutOfBoundsException with # in JVM parameters
  * fix [issue 73](https://code.google.com/p/javamelody/issues/detail?id=73): / by zero when max\_elements\_in\_memory = 0 in ehcache
  * fix [issue 74](https://code.google.com/p/javamelody/issues/detail?id=74): "View OS Processes" does not work on MAC OS X Server
  * fix [issue 75](https://code.google.com/p/javamelody/issues/detail?id=75): StringIndexOutOfBoundsException for http session with # in attributes
  * added: in the optional collect server, "add and remove applications" can be restricted ([issue 61](https://code.google.com/p/javamelody/issues/detail?id=61))
  * added an enhancement on jdbc proxy: Improve JDBC connection wrapper equals invocation when `args[0]` is a javamelody proxy ([issue 78](https://code.google.com/p/javamelody/issues/detail?id=78))

  * added: In the system actions, new view "**MBeans**" with the values and the descriptions of the attributes. (MBeans contain configuration and low-level data on the application server and on the JVM). The values are viewable but not writable and operations can not be performed.

  * added for **[Munin](http://munin.projects.linpro.no/)/[Nagios](http://www.nagios.org/)**: for each MBean attribute, a link on the left of the attribute is provided to have only the current value of an attribute in plain text. And a link below zoomed graphics is also provided to have only the last value in the graphic. For example, the following URLs can be used in this version: [1](http://localhost:8080/context/monitoring?part=lastValue&graph=usedMemory,cpu) and [2](http://localhost:8080/context/monitoring?jmxValue=Catalina:type=GlobalRequestProcessor,name=http-8080.bytesSent|java.lang:type=OperatingSystem.ProcessCpuTime). (To use the url for a MBean value, the system actions must be [enabled](UserGuide#6._Optional_parameters.md)).<br />This feature can probably be used to write a Munin/Nagios plugin using something like [this](https://github.com/coderholic/munin-popularity-plugins) in order to have in Munin the same graphics as in JavaMelody or to have some new graphics based on MBeans values.

  * added in the [Hudson Monitoring plugin](http://wiki.hudson-ci.org/display/HUDSON/Monitoring): **Monitoring of the Hudson nodes** (slaves in general) similar to the monitoring of the Hudson master.<br />If the monitoring of the Hudson master is available at `http://localhost:8080/monitoring` then the monitoring of the Hudson nodes is available at `http://localhost:8080/monitoring/nodes`.<br /><br />The monitoring of the Hudson nodes includes:
    * Aggregated "used memory" chart, aggregated "% cpu" chart (between 0 and 100) for day, week, month, year or a custom period.
    * Chart of the build times over time for the selected period
    * Other aggregated charts: % GC, threads count, loaded classes count, system load average, open file descriptors count and more for the selected period
    * Statistics of the build times for the selected period
    * Running builds with time elapsed
    * Threads informations for each node including name, state, stack-trace and an action to kill any thread
    * Heap histogram aggregated for all nodes
    * Current system informations for each node
    * MBeans for each node
    * Last values in charts and MBeans values like above. For example, the following URLs can be used: [1](http://localhost:8080/monitoring/nodes?part=lastValue&graph=usedMemory,cpu) and [2](http://localhost:8080/monitoring/nodes?jmxValue=java.lang:type=OperatingSystem.SystemLoadAverage|java.lang:type=OperatingSystem.ProcessCpuTime)
    * Process informations for each node
    * Actions to execute the GC or to generate a heap dump on each node

  * Due to the current infrastructure changes for the [maven2-repository in java.net](https://maven2-repository.dev.java.net):
    * The **Monitoring plugin 1.24.0 for Hudson** can be downloaded from http://javamelody.googlecode.com/files/monitoring.hpi<br />You can submit this hpi file in the Advanced tab of the plugin manager in Hudson and restart the server.
    * The jar file **javamelody-core 1.24.0 is not published in any maven repository**.
    * A new release will probably be made when the [maven2-repository](https://maven2-repository.dev.java.net) comes back.

### 1.23.0 ###

  * fix [issue 66](https://code.google.com/p/javamelody/issues/detail?id=66): Since tomcat 6.0.21 and when tomcat based authentication is used, session count is false and there is a possible memory leak for invalidated http sessions. This is because of the changes for tomcat enhancement [45255](https://issues.apache.org/bugzilla/show_bug.cgi?id=45255). To tomcat experts: Isn't an http session supposed to die first before being able to born a second time? (Update: see [51042](https://issues.apache.org/bugzilla/show_bug.cgi?id=51042))
  * minor change in the gzip compression of the reports in order to fix the compatibility issue between the javamelody grails plugin and the UiPerformance grails plugin.

  * The "Monitoring" plugin can be installed by point and click in the plugin manager of a Hudson server, or it can be downloaded from http://download.hudson-labs.org/plugins/monitoring/. The plugin manager says that it is built for Hudson 1.388, but in fact it works for older versions of Hudson.

### 1.22.0 ###

  * fix [issue 54](https://code.google.com/p/javamelody/issues/detail?id=54): v1.21.0 requires jdk 1.6, when "displayed-counters" is used
  * fix [issue 58](https://code.google.com/p/javamelody/issues/detail?id=58): Quartz in Spring and server shutdown. (Added parameter "quartz-default-listener-disabled" to be used for Quartz jobs only when used in Spring. See [doc](UserGuide#12._Batch_jobs_(if_Quartz).md).)
  * fix [issue 59](https://code.google.com/p/javamelody/issues/detail?id=59): Maximum values in statistics can be incorrect
  * fix [issue 62](https://code.google.com/p/javamelody/issues/detail?id=62): IndexOutOfBounds for report on some jobs
  * added major feature for **Hudson and Jira/Confluence/Bamboo plugins**: graphic of the number of sessions and details on http sessions with the link in "System informations" (this is [emulated](http://code.google.com/p/javamelody/source/browse/trunk/javamelody-core/src/main/java/net/bull/javamelody/PluginMonitoringFilter.java) without httpsessionlistener, because httpsessionlistener can't be used in Hudson plugins or in Atlassian plugins)
  * added: @MonitoredWithSpring can be added on interfaces like on implementation classes. See [doc](UserGuide#9._Business_facades_(if_Spring).md).
  * added: the system property -Djavamelody.plugin-authentication-disabled=true can be added to a Hudson, JIRA, Confluence or Bamboo server in order to disable authentication of the monitoring page in the javamelody plugin and to be able to add the server to a centralized collect server. See [doc](UserGuideAdvanced#4._Results_with_a_collect_server.md). (A system property like -Djavamelody.allowed-addr-pattern=127\.0\.0\.1 can also be added with the ip address of the collect server)
  * removed: because parameters in environment variables can't work well in all OS, the possibility to use environment variables is removed. Use [this](UserGuide#6._Optional_parameters.md) instead.

### 1.21.0 ###

  * fix [issue 48](https://code.google.com/p/javamelody/issues/detail?id=48): Quartz errors for persistent jobs after jobs' classes are renamed or deleted
  * fix to display the list of http sessions when Tomcat throws an exception "Session already invalidated"
  * fix to display the list of process when using Windows in Germany
  * added: If Tomcat or JBoss, new graphics in "Other charts" for the number of active http and ajp threads in all the server, the number of bytes received per minute and the number of **bytes sent per minute** by the server (the number of bytes received is often 0 for most webapps).
  * New view with the **summary per class of statistics** for EJB/Spring/Guice/services with aggregation for methods and another view for the methods of the selected class.
  * enhanced [documentation](UserGuideAdvanced#2._Deployment_of_the_webapp_of_monitoring.md) on the configuration of the optional centralization server such as to enable system actions or to send daily/weekly/monthly reports by mail.

### Grails Plugin 0.5 ###

[JavaMelody Grails Plugin](http://www.grails.org/plugin/grails-melody) 0.5 has been released.

  * includes JavaMelody 1.20.0
  * Grails version was updated to 1.2.4 in the plugin, only to be able to release the plugin
  * [issue 36](https://code.google.com/p/javamelody/issues/detail?id=36) is a known open issue for the plugin when a closure is used in a service (contribution welcome)

### 1.20.0 ###

  * added: **German translation** thanks to _Ewald Arnold_. We would like to have [feedback here](http://groups.google.fr/group/javamelody/browse_thread/thread/5656e63513436b64).
  * added: Documentation on using JavaMelody with a **[maven dependency](UserGuide#Dependencies.md)**
  * fix: In the html display of the **current requests**, put back the complete http request with query parameters and values, like it was displayed before v1.16.0
  * fix [issue 45](https://code.google.com/p/javamelody/issues/detail?id=45): **Optimization** of html rendering when there are thousands of threads or current requests: the threads are displayed in their own page when there are more than 500 threads, and the "current requests" are also displayed in their own page when there are more than 500 of them. If there are less than 500 threads or current requests, then nothing is changed in the reports.
  * Note that a webapp with JavaMelody has no problem with 7000 threads and 7000 active http requests ! and that html reports are already highly compressed in this case.
  * For the same optimization reason, when the optional [centralized collect server](UserGuideAdvanced#Optional_centralization_server_setup.md) is used, the data from the monitored application to the collect server are now compressed when the size of the stream is larger than 50 KB (for example, when there are 7000 threads). This applies when the transport format is 'serialized' but also when it is 'xml' (or 'json').

### 1.19.0 ###

  * fix [issue 38](https://code.google.com/p/javamelody/issues/detail?id=38): Distributed (XA) Transactions aborted by 'rollback' in JavaInformations.java
  * fix [issue 41](https://code.google.com/p/javamelody/issues/detail?id=41): NumberFormatException requesting process information
  * fix [issue 44](https://code.google.com/p/javamelody/issues/detail?id=44): Sql requests not monitored when using jdk1.6, a recent jdbc driver and net.bull.javamelody.JdbcDriver
  * added: **[Alternative for sql requests monitoring](UserGuideAdvanced#Alternative_for_monitoring_of_sql_requests.md)** when jdbc datasource or jdbc driver can't be used and when Hibernate is used: HibernateBatcherFactory
  * **[Logs for debugging](UserGuideAdvanced#Debugging_logs.md)** added using either logback if found, or log4j if found or java.util.logging, and using the category "net.bull.javamelody" and the level "debug". Moreover, existing stack-traces of exceptions are no longer printed in the error console of the server, and they are now logged in logback or log4j or java.util.logging, using the category "net.bull.javamelody" and the level "info" or "warn". Logging of http requests is unchanged.
  * added: Names of **[MonitoringProxies](UserGuideAdvanced#Business_facades_%28without_EJB3_and_without_Spring_and_without_Gu.md)** can now be customized.
  * added: [Documentation](UserGuide#6._Optional_parameters.md) on the javamelody parameter `no-database`
  * added: [Documentation](UserGuideAdvanced#Report_written_before_last_shutdown.md) on the report written before the last shutdown

### 1.18.0 ###

  * fix [issue 33](https://code.google.com/p/javamelody/issues/detail?id=33): Database name or jdbc driver with '#' causes IndexOutOfBounds
  * fix [issue 34](https://code.google.com/p/javamelody/issues/detail?id=34): Application server restart causes persistent Quartz jobs to be hanging around
  * added: "JNDI Tree" link in "system actions" (for example to check if a datasource is really in "java:/" or in "java:/comp/env/")
  * install note for bamboo 2.6: see end of [issue 37](https://code.google.com/p/javamelody/issues/detail?id=37)

### Grails Plugin 0.4 ###

[JavaMelody Grails Plugin](http://www.grails.org/plugin/grails-melody) 0.4 has been released by _Liu Chao_.

  * includes JavaMelody 1.17.0, in particular for Quartz jobs

### 1.17.0 ###

  * fix [issue 28](https://code.google.com/p/javamelody/issues/detail?id=28): NoSuchMethodError when undeployed and when JavaMelody 1.15.0 or 1.16.0 is used with Quartz 1.7 or 1.8
  * fix [issue 30](https://code.google.com/p/javamelody/issues/detail?id=30): Quartz job CRON expression with '#' causes NullPointerException
  * fix for Grails: an exception in initialisation of Quartz should not stop initialisation of the webapp
  * fix [issue 8](https://code.google.com/p/javamelody/issues/detail?id=8): A synchronized (rrdDb) was added against the exception "Bad sample timestamp x. Last update time was x, at least one second step is required". It was seen with deployment on Tomcat without modification of monitored webapps.
  * fix for JIRA: "mean sql hits" and "mean sql time" displayed in http and in jsp statistics
  * changed: The default order of MonitoringSpringAdvisor is now 0 for priority before other spring advisors ([issue 32](https://code.google.com/p/javamelody/issues/detail?id=32))
  * added: Statistics of **[Struts 2](http://struts.apache.org/)** actions
  * added: If Oracle database and if system actions are enabled, 3 new reports on foreign keys without indexes, invalid objets and disabled constraints

### 1.16.0 ###

  * Compatibility of the [JIRA/Bamboo plugin](https://plugins.atlassian.com/plugin/details/20909) with **Confluence**
    * Notes: The "Monitoring" link is moved to the Administration menu and only administrators in JIRA/Bamboo/Confluence are authorized to view reports. Http sessions are unfortunately never seen by this plugin.
  * Monitoring of jdbc connections and of **sql requests in JIRA** (and in Confluence and Bamboo if a jdbc datasource is used and not a direct jdbc connection)
  * Fix [issue 26](https://code.google.com/p/javamelody/issues/detail?id=26): In JIRA, the count of used jdbc connections increases in the display with each "/monitoring" page refresh (but connections are really closed)
  * added: Monitoring of Business facades with **[Guice](http://code.google.com/p/google-guice/)** (like with EJB or with Spring)
  * Some minor bugs fixed

### JavaMelody Grails Plugin 0.3 ###

[JavaMelody Grails Plugin](http://www.grails.org/plugin/grails-melody) 0.3 has been released by _Liu Chao_.

  * fix [issue 18](https://code.google.com/p/javamelody/issues/detail?id=18): Grails-melody plugin breaks the filterpane plugin
  * includes JavaMelody 1.15.0

### 1.15.1 of Hudson plugin and of JIRA/Bamboo plugin ###

  * **[JIRA/Bamboo plugin](https://plugins.atlassian.com/plugin/details/20909)**
    * fix [issue 24](https://code.google.com/p/javamelody/issues/detail?id=24): StackOverflow when a SSO plugin with seraph is used
    * jdbc connections and sql requests are now monitored in JIRA (like jsp pages)

  * **[Hudson plugin](http://wiki.hudson-ci.org/display/HUDSON/Monitoring)**
    * change to reduce disk usage: Some common http requests are now aggregated in statistics, for example on build numbers. The javamelody.http-transform-pattern parameter has now the default value of ` "/\d+/|/site/.+|avadoc/.+|/ws/.+|obertura/.+|estReport/.+|iolations/file/.+|/user/.+|/static/\w+/" ` and all matches in http URLs will be replaced by "$". Note that it is possible on each Hudson server to change the value of this parameter with a system property -Djavamelody.http-transform-pattern=xxx in the java command line.

### 1.15.0 ###

  * fix [issue 20](https://code.google.com/p/javamelody/issues/detail?id=20): SQL requests were not monitored and "jdbc connections" were always 0, when **JndiObjectFactoryBean** is used in Spring
  * fix [issue 21](https://code.google.com/p/javamelody/issues/detail?id=21) for **JIRA**: Null pointer with quartz jobs in jira
  * fix [issue 22](https://code.google.com/p/javamelody/issues/detail?id=22) for **JIRA**: The report will be visible only for jira administrators
  * fix [issue 23](https://code.google.com/p/javamelody/issues/detail?id=23) for **JIRA**: The report displayed only random characters when gzip compression is enabled in jira
  * [doc updated](UserGuide#10._Batch_jobs_(if_Quartz).md) for **quartz** jobs scheduled with Spring, with help from _Pether Sörling_.
  * [doc updated](UserGuide#3._First_results.md) for [issue 19](https://code.google.com/p/javamelody/issues/detail?id=19) InternalError "Can't connect to window server" on Mac OS X Server.
  * [doc updated](UserGuide#14..md) for usage of JavaMelody when SecurityManager is enabled
  * added: Statistics of rendering times in **JSP pages** (when RequestDispacher is used)
  * added: If **quartz** is available, display of mean time and last error with stack-trace per job for the selected period, and also display of the statistics for all the jobs with number of executions, mean cpu times, number of sql hits, sql times and last 100 errors with stack-traces for the selected period.
  * added: In Hudson plugin, if the server has servlet api 3.0 (JavaEE 6), then display the graphic of the number of http sessions and display the list of current http sessions
  * change: the graphics and the statistics of [ejb](UserGuide#8._Business_facades_(ejb-jar.xml_file_if_EJB3).md), [spring](UserGuide#9._Business_facades_(if_Spring).md) and [services](UserGuideAdvanced#Business_facades_(without_EJB3_and_without_Spring).md) will be displayed automatically when they are used (except if the parameter "displayed-counters" has been defined AND if the parameter does not include "ejb", "spring" or "services"). So usage of ejb, spring or services will be easier because it will not be necessary anymore to define the parameter "displayed-counters".
  * change to reduce disk usage: graphics for "http system errors" and "system errors logs" are no longer displayed (unneeded files will be automatically deleted at midnight)

### 1.14.0 ###

  * fix: Use **spring datasources** in some parts of the reports, if of course the monitoring-spring.xml file (with the post-processor) is used as said in the [user guide](UserGuide#7._JDBC.md). Like with jndi datasources, database version and jdbc driver version will be displayed. And if system actions are enabled, reports on the database will be available (like mysql variables and process list, or oracle long requests).
  * fix [issue 16](https://code.google.com/p/javamelody/issues/detail?id=16): InternalError "Unable to open directory /proc/self/fd" on ubuntu or debian using the tomcat package with jsvc
  * fix in the javamelody plugin for atlassian (jira, bamboo, confluence): better aggregation of http URLs and less memory overhead in jira and in bamboo (the parameter "http-transform-pattern" has been changed in the atlassian plugin from "-\d+" to "-\d+|chment/.+|onent/.+|est/.+|ifact/.+" for attachment, component, test, artifact)
  * added with help from _Pether Sörling_: If **Logback** is available, warn logs are automatically monitored in order to display them in reports like with log4j and with java.util.logging, and Logback is used for log of http requests.
  * added: if system actions are enabled, display of stack traces of where were opened jdbc connections, in order to help **find jdbc connection leaks**

Atlassian JIRA and Bamboo plugin has been published on Atlassian plugin exchange: https://plugins.atlassian.com/plugin/details/20909

### 1.13.0 ###

  * fix [issue 14](https://code.google.com/p/javamelody/issues/detail?id=14): html reports in utf-8 for cyrillic and other characters
  * fix [issue 15](https://code.google.com/p/javamelody/issues/detail?id=15): support for log4j-over-slf4j
  * added: remember the last selected period (with a persistent cookie in the browser)
  * added: UI option to display graphs and statistics for **custom periods**, via fields of start and end dates of period to display (from user group)

### 1.12.0 ###

  * if a centralized collect server is used:
    * fix [issue 12](https://code.google.com/p/javamelody/issues/detail?id=12): workaround for a bug in Winstone (standalone mode)
    * fix [issue 13](https://code.google.com/p/javamelody/issues/detail?id=13): NoClassDefFoundError with ehcache in the collect server if the monitored application uses ehcache
  * added: If Oracle database, display of **sql execution plan** in sql request detail (seems not doable if postgresql or mysql, without particular parameter values)
  * added: **New charts** "Threads count", "Loaded classes count", "Used non heap memory", "Used physical memory", "Used swap space" (displayed with the new link "Other charts")
  * added: spring post-processor in net/bull/javamelody/monitoring-spring.xml to auto-detect datasources defined in a spring xml file.
  * added: If quartz is available in classpath, display of the **list of the [quartz](http://www.quartz-scheduler.org/) jobs** with date and hour of previous execution, date and hour of next execution, duration of running jobs and buttons to pause and to resume all jobs or each job
  * added: Button to kill a java thread
  * added: New parameter `monitoring-path` to change the url "/monitoring" of the report to "/admin/monitoring" for example (from user group)

### Atlassian JIRA plugin ###

A beta version of a JIRA plugin is released now to integrate JavaMelody in Atlassian JIRA.
See [UserGuide](UserGuide.md) for installation.

This plugin has been tested with a JIRA 4.0.1 server, but it certainly works with Confluence and Bamboo.

### 1.11.1 ###

  * fix for AssertionError in sql counter when assertions are enabled (in maven surefire for example)
  * if a centralized collect server is used:
    * fix: after an action like GC is called do not execute the action again and again when the page is refreshed with the action parameter in the url
    * added: display of a message to say what was done after an action is called
  * added: in sql or spring or ejb request detail, link to display the reversed call-tree (i.e. usages) of the request
  * added: parameters can now be specified in environment variables like in system properties, webapp context or as init-param of MonitoringFilter
  * added: if postgresql, display of pg\_locks, pg\_tablespace, pg\_database and of cachehitratio in pg\_stat\_database
  * added: if ehcache, an action is available to clear all caches of all cache managers
  * added: extraction of "maxActive" and other properties from apache dbcp BasicDataSource like what is already extracted from apache tomcat BasicDataSource (as discussed in user group)
  * added: new parameter "mail-periods" to change the period of mail reports from weekly to daily or monthly or a combination of the 3 (as suggested in user group)
  * added: display the version of JavaMelody at the bottom of the html and pdf reports (as suggested in user group)
  * added: [documentation](UserGuideAdvanced#Usage_of_in_JonAS_5_(which_uses_OSGI).md) to use JavaMelody in JonAS 5
  * added: display of "ajax GET" or "ajax POST" in http requests names for ajax requests
(same as of version 1.11.0 except for a java assertion which has no impact in general)

### Grails Melody plugin ###

A Grails Melody plugin was released by Liu Chao at http://www.grails.org/plugin/grails-melody to integrate JavaMelody in Grails (Groovy on rails).

### 1.10.0 ###

  * fix: added automatic monitoring of jdbc datasources when their jndi names starts with java:/jdbc like when they starts with java:comp/env/jdbc (for jboss for example)
  * change: default language is now English. To help people outside US, UK and France: the language of the UI is always based on the language of the browser if a translation is found (just 'fr' for now), or now on the language of the java server if a translation is found, or to the default translation (that is now English).
  * added: [documentation](UserGuideAdvanced.md) to setup JavaMelody in an EAR file (based on a contribution by 'dhartford')
  * added: [documentation](DevGuide.md) to import current development sources in eclipse
  * added: memory overhead estimated in reports

### 1.9.0 ###

  * fix [issue 3](https://code.google.com/p/javamelody/issues/detail?id=3) "http status 500" : NoSuchElementException in getPID on Solaris 10
  * fix for use of JavaMelody in tomcat's lib directory without modification of monitored webapps (fix NullPointerException in dbcp datasource), [documentation](UserGuideAdvanced#Deployment_on_Tomcat_without_modification_of_monitored_webapps.md) added
  * added: display of availability or unavailability of applications in the collect server
  * added: [documentation](UserGuideAdvanced.md) for monitoring of services without ejb3 and without spring

### 1.8.2 ###

  * fix ClassCastException for ehcache v1.2.1 to v1.2.4 (thanks hl)
  * fix: c:\... is now an absolute path for parameter "storage-directory" (and not relative to temp)
  * sql monitoring by jdbc datasource can now be disabled by an empty "datasources" parameter
  * added: display of deadlocks between threads if there are
This is the same version of JavaMelody as in the hudson plugin "[Monitoring](http://wiki.hudson-ci.org/display/HUDSON/Monitoring)" available in [hudson](http://hudson-ci.org/).

### 1.8.0 ###

  * fix for sql monitoring : the sql requests were monitored only in tomcat when a jdbc datasource was used. Fix tested with tomcat v5.5 & v6, glassfish v3, jboss v5, weblogic v11g, jetty v6, jonas v5
  * fix for container authentication on the report (thanks hasalex)


### 1.7.0 ###

> Initial release on javamelody.googlecode.com