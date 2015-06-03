## TODO list of features ##

Here is a list of wishes of features.

JavaMelody is opensource:
Priorities and todos may change any time.
There is no promise of dates or that a particular feature will ever be done.

Follow JRobin issues:
  * ~~http://issues.opennms.org/browse/JRB-20,~~ (done)
  * http://sourceforge.net/tracker/?func=detail&aid=3403733&group_id=82668&atid=566807,
  * ~~and http://sourceforge.net/mailarchive/message.php?msg_id=28038427 (one issue submitted in JIRA: http://issues.opennms.org/browse/JRB-26)~~ (done in 1.46, [issue 96](https://code.google.com/p/javamelody/issues/detail?id=96))

And
  1. Fix [issues](http://code.google.com/p/javamelody/issues/list)
  1. More unit tests
  1. More integration tests (all JDK/JRE and versions, all OS, all application servers and versions, all navigators)
  1. **Translations** (Italian, Spanish...)
  1. ~~Add an action for threads dump as text format~~ (done)
  1. ~~In the collector server, refuse to add twice one instance of an application (for example, individually and then in a cluster), because it would not work~~ (done)
  1. If the transaction isolation level is not read committed (unusual), display a warning about slowness or bugs
  1. ~~Add pdf/xml/json reports for the counter summary per class~~ (done)
  1. Display the 100 latest slow requests for each counter (duration at severe level when they were executed), including user and http parameters
  1. ~~Sampling of thread stack traces to detect cpu hotspots (similarly to VisuaVM sampler but with a lower rate and with no configuration). The packages java, javax, sun, com.sun should be excluded. (And for memory hotspots?).~~ (done in 1.47, [issue 149](https://code.google.com/p/javamelody/issues/detail?id=149))
  1. ~~In the collector server, do not display all the applications at the top if there are more than 10 of them, and display them with a show and collapse link~~. (done)
  1. Cannot remove in the GUI an application from the collector server when the application is no longer using javamelody. When trying to select it, the collector server is showing an error 500 with the message "Data unavailable for the application". (Meanwhile, it is possible to remove the application directly from the applications.properties file in the temp directory.)
  1. Inline drill-down in child requests, for example in the detail of a request
  1. Add a pdf report of the detail of a request
  1. Add the 90th or 95th percentile in zoomed graphics, besides the mean (org.jrobin.data.DataProcessor.getPercentile)
  1. ~~Stack-traces: Link to open a popup with the stack-trace (in addition to the tooltip)~~ (canceled, there is an action "Dump threads as text" now)
  1. Monitor ~~incoming or~~ outgoing web-services when using Apache CXF or JavaEE7-JAX-RS2. See [Interceptors](http://cxf.apache.org/docs/interceptors.html) or [this one](http://code.google.com/p/xebia-france/wiki/CxfMonitoring#Spring_configuration) for CXF and [this one](http://www.oracle.com/technetwork/articles/java/jaxrs20-1929352.html) for JAX-RS2 (incoming web-services done with [issue 228](https://code.google.com/p/javamelody/issues/detail?id=228))
  1. ~~Test the compatibility or the non-compatibility with Quartz 2.0~~ (done, see [issue 121](https://code.google.com/p/javamelody/issues/detail?id=121))
  1. ~~Test the monitoring of sql requests with a datasource from jndi in **[Glassfish 3](https://groups.google.com/group/javamelody/t/39da9d1d1fe5f275)**~~ (done, see [issue 138](https://code.google.com/p/javamelody/issues/detail?id=138))
  1. Test the monitoring of sql requests with a datasource from jndi in **Resin**
  1. ~~Test the jndi report in **Websphere**~~ (done)
  1. Option in the UI to change from cumulative times (http times include sql) to **non-cumulative times**, via a link and a persistent http cookie or at least a parameter (from user group)
  1. ~~Downloadable **demo** of JavaMelody in [java pet store](http://java.sun.com/developer/releases/petstore/) with step by step instruction to install in glassfish or [Spring pet clinic](http://static.springsource.org/docs/petclinic.html) in tomcat for example~~ (done, there is an [online demo](Demo.md).)
  1. Basic server monitoring: in [downloads](https://github.com/javamelody/javamelody/releases), provide a simple war file to monitor only an application server (cpu, memory, system informations, mbeans, threads) without monitoring any application (http requests, http sessions, etc)
  1. Cloud Watch metrics for an application in Amazon EC2, possibly by using Beans Talk.
  1. ~~Statistics of JSF actions like for Struts actions~~ (done)
  1. ~~Possibly document the external API: [lastValue and jmxValue](ReleaseNotes#1.24.0.md) ; and also xml and json format on some pages~~ (done)
  1. Possibly document the rewrap-datasources parameter for Tomcat and Jonas
  1. Format the sql requests before display in the sql statistics
  1. Add a button to kill a system process and its children in the process list?
  1. Monitoring of [nio buffers](http://blogs.oracle.com/alanb/entry/monitoring_direct_buffers) if Java 7 and of the new [% system cpu](http://download.oracle.com/javase/7/docs/jre/api/management/extension/com/sun/management/OperatingSystemMXBean.html#getSystemCpuLoad%28%29) if Java 7 and Oracle JDK (in other charts?). getSystemCpuLoad may not be implemented on Windows (returns -1) and getProcessCpuLoad is probably useless given that there is already the "% cpu" for the process.
  1. Add a view with the classloader hierarchy
  1. Simplify UI with [user feedback](http://groups.google.fr/group/javamelody/browse_thread/thread/d4ac1b796e366852)
  1. Details on GC types and on cpu per gc type (from user group)
  1. Display of graphs with **percentages of navigators, of OS, of countries/languages** based on user-agent and main locale, with jrobin and percentage one above the others up to 100%, and idem with types of requests (get,post,ajax,gwt or perhaps select/update/insert/delete)
  1. **Alerts** based on thresholds, with mails to administrators
  1. ~~**CSV** exports of tables~~ (done in the rich desktop UI. Note that there are also [XML and JSON](ExternalAPI.md) exports possible in the web UI)
  1. ~~Rich **desktop UI**, with [swing](http://java.sun.com/docs/books/tutorial/uiswing/) and [java web start](http://java.sun.com/javase/technologies/desktop/javawebstart/index.jsp)~~ (done, [ReleaseNotes#1.42.0](ReleaseNotes#1.42.0.md))
  1. **[Munin](http://munin.projects.linpro.no/)/[Nagios](http://www.nagios.org/)/[Zabbix](http://www.zabbix.com)** plugin to display some graphs in those tools
  1. ~~Add the Nagios plugin by Shawn Bower in the user guide in addition to the [External API](ExternalAPI#PNG_and_lastValue.md), if it works well.~~ (done)
  1. When monitoring-spring.xml is used (or monitoring-spring-aspectj.xml), add a page to display beans in the Spring context by adding a bean which implements ApplicationContextAware
  1. Add a plugin interceptor to monitor sql requests in mybatis when no other sql monitoring, such as a datasource from JNDI, is used
  1. Play framework, see [stackoverflow](http://stackoverflow.com/questions/7409112/javamelody-and-play-framework)
  1. Extract informations from bonecp such as max connections like from dbcp
  1. Custom processing in counters's events ([issue 173](https://code.google.com/p/javamelody/issues/detail?id=173))
  1. Possibility to add graphics based on configurable MBeans values (such as business metrics)
  1. In the list of dependencies of the webapp, add a link to a new page to display for each dependency the name with a link to the dependency's url, Maven ID with groupId:artifactId:version, the name of the license with a link to its URL (like Jenkins)
  1. In the list of Quartz jobs, if a job is an InterruptableJob and is currently executing, display a red cross next to the progress bar (like Jenkins jobs) to be able to [interrupt](http://grepcode.com/file/repo1.maven.org/maven2/org.quartz-scheduler/quartz/2.1.3/org/quartz/Scheduler.java#Scheduler.interrupt%28org.quartz.JobKey%29) the job in [QuartzAdapter](http://code.google.com/p/javamelody/source/browse/trunk/javamelody-core/src/main/java/net/bull/javamelody/QuartzAdapter.java) and [Quartz2Adapter](http://code.google.com/p/javamelody/source/browse/trunk/javamelody-core/src/main/quartz/net/bull/javamelody/Quartz2Adapter.java).
  1. Having some monitoring is good but sometimes not enough or too late in the development cycle (and sending alerts are not better for developers). A parameter such as "max-sql-hits" could be added for dev environments, for continuous integration tests or for QA servers, in order to enable a hard rule: no more than X executions of SQL requests per service execution or per http request execution, otherwise throw an exception immediately like a blocking issue. A good practice would be X=10, or X=20 for example can be chosen to be "comprehensive" for some time.
  1. ~~If the "sql-transform-pattern" parameter is used, the sql requests may contain the '$' character, and then the execution plan (with Oracle) does not work on these requests (improved in 1.51, [revision 3799](https://code.google.com/p/javamelody/source/detail?r=3799))~~
  1. Check with a file if 2 instances use the same storage directory (and warn about data corruption)
  1. ~~Add a pdf report for the JNDI bindings~~ (done in [revision 3101](https://code.google.com/p/javamelody/source/detail?r=3101))
  1. ~~Add a Table Of Contents with links at the top of the main report. In order to not take much vertical space, it can be displayed hovering, or in a combo box, or in a popup...~~ (done in 1.50, [revision 3705](https://code.google.com/p/javamelody/source/detail?r=3705))
  1. ~~Create a plugin for Liferay v6.1 or later~~ (done, [doc](UserGuide#Liferay_Plugin.md))
  1. **Customized reports for each version of the monitored application, to ease the comparison of statistics between versions:** There could be an optional javamelody parameter giving the current application version. When the developer changes the parameter's value, a properties file could be stored with the list of the application's versions including start and end dates. And so javamelody would be able to display the list of the application's versions with links to display the reports with customized periods. Otherwise, that list and the links to customized reports can be manually maintained in a wiki for example, after each deployment of the application. New 'comparison' reports may also be displayed to show comparison statistics/details between two, or across all, application versions.  Potentially this could initially be limited to a selected statistic marker, and/or only as an aggregated general report (for usecase of showing an aggregated report of 'version 5 is 10% faster than version 4'). This can be extended to compare the statistics in the details of requests, with drill-downs.
  1. **Display graphics of GC in some specific screen(s) when GC logs are enabled**. In the meantime, [HPJMeter](http://www.hp.com/go/hpjmeter) can be used.
  1. ~~In the [Jenkins Monitoring plugin](https://wiki.jenkins-ci.org/display/JENKINS/Monitoring), if Jenkins v1.502+, add a [BuildStepListener](http://javadoc.jenkins-ci.org/hudson/model/BuildStepListener.html) as an extension point, to display build steps with mean execution and cpu times, in the details page of the statistics of a build. The name of the build steps should include the name of the job + " / " + Describable.getDescriptor().getDisplayName() or class.getSimpleName() + if that's the case, ant targets, maven goals or shell command.~~ (done, except specifics of build steps: ant targets, maven goals, shell command).
  1. ~~In JIRA/Confluence/Jenkins/Liferay for example, the user of the monitoring reports is often authenticated and has some http session: so a "Logout" action could be added in the menu on the right of the main report.~~ (done for 1.52)
  1. ~~In Jenkins, add the possibility to call "System actions" for each slave individually [JENKINS-20935](https://issues.jenkins-ci.org/browse/JENKINS-20935)~~ (done for 1.49)
  1. Heap and PermGen objects explorer using JDI. See links [1](https://github.com/puneetlakhina/javautils/blob/master/src/com/blogspot/sahyog/PrintStringTable.java) and [2](http://stackoverflow.com/questions/2842982/how-to-analyze-permgen-contents) for example (D. Hartford)
  1. In the monitored servers, report anonymous data to a central server. This would need to show openly that only anonymous data is collected (java version, os, #cores, heap size...) and that it can be disabled. This would enable several interesting things:
    * Know after all how many active installs there are
    * Report anonymous open-data about users (% of melody versions, % of java versions...)
    * Social gaming between willing users, with experience gained over time, fan badges and heroes sets of powerful weapons and shields to share their experiences, bonus points when more than 1000 sql hits per http request, etc
  1. Maven:
    1. ~~Why **maven** waits forever to build in Eclipse when I have forgotten to change the proxy in my settings.xml between my work day and my work night ? and why can't I cancel the waiting build ?~~ (I don't have to use a proxy at the moment)
    1. ~~And why some maven plugins are sometimes years late?~~ (obsolete, olamy took care of this)
    1. ~~Publish JavaMelody in maven central repository (seems not doable without jrobin in central repo and without renaming package to googlecode)~~ (done, it is in maven central now.)
  1. Think about [hosting](http://docs.codehaus.org/display/SONAR/Hosting+on+the+Forge) the [sonar javamelody plugin](https://github.com/evernat/sonar-javamelody) as suggested by [Ann's message](http://sonar.markmail.org/message/myzjbtuxvum74ppm?q=list:org.codehaus.sonar.dev) and so publish in all Sonar's update centers.
  1. In the list of http sessions, the city or country could be displayed for each session based on IP address of the client. A link to an online map can be given using latitude and longitude. A free [database](http://dev.maxmind.com/geoip/geoip2/geolite2/) could be downloaded or an [online service](https://code.google.com/p/geoip-appengine/) could be used for that.
  1. In the database informations report, be able to switch between DB when there are several, instead of displaying only the report for the first DB.
  1. When a http request is very very long without white-space, it may cause the statistics table to be much wider than the screen. The width of that column should be auto according to the content, but with a max.
  1. More to come...