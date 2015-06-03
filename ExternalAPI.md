This page documents the external API of JavaMelody, including how to get the monitoring data in a XML format.

## Table of contents ##

  * [HTML](#HTML.md)
  * [PDF](#PDF.md)
  * [PNG and lastValue](#PNG_and_lastValue.md)
  * [jmxValue](#jmxValue.md)
  * [XML](#XML.md)
  * [JSON](#JSON.md)
  * [Serialized](#Serialized.md)
  * [API when using a collect server](#API_when_using_a_collect_server.md)

### HTML ###

The monitoring pages can display monitoring data in the HTML format.

For example, for the day period you can use the URL:
http://localhost:8080/mywebapp/monitoring?period=jour
where you can replace localhost:8080 and mywebapp with the host, port and context of your server.

You can also replace the "period" parameter with "semaine", "mois", "annee" and "tout" for the week, month, year and all periods. Or you can use a custom period.

Other pages are available as you can see in those monitoring pages.

For example: http://localhost:8080/mywebapp/monitoring?part=counterSummaryPerClass&counter=guice&period=jour

The "part" parameter can be "threads", "currentRequests", "counterSummaryPerClass" and if system actions are [enabled](UserGuide#6._Optional_parameters.md) (true by default), it can be "heaphisto", "sessions", "web.xml", "mbeans", "processes", "jndi", "connections" or "database".


### PDF ###

The monitoring data is also available in the PDF format if the [iText dependency](UserGuide#Dependencies.md) is available in the webapp.

For example, for the day period you can use the URL: http://localhost:8080/mywebapp/monitoring?format=pdf&period=jour

And you can also change the "period" parameter like for the HTML format.

Other PDF reports are available. For example:
http://localhost:8080/mywebapp/monitoring?format=pdf&part=runtimeDependencies&counter=spring&period=jour

The "part" parameter for the PDF format can be replaced in the URL with "counterSummaryPerClass", "runtimeDependencies", "heaphisto", "sessions", "mbeans", "processes" or "database".


### PNG and lastValue ###

For each graphic displayed, it is possible to retrieve a PNG image of the graphic with the width and the height that you want.

For example: http://localhost:8080/mywebapp/monitoring?graph=usedMemory&width=960&height=400&period=jour

It is also possible to get as plain text the last value added in the graphic. A link below zoomed graphics is provided for this.

For example, the following URL retrieves the last measured "used memory" and "% cpu" as "1.0622728E7,0.0":
http://localhost:8080/mywebapp/monitoring?part=lastValue&graph=usedMemory,cpu

Usual graphics names include:
  * usedMemory, cpu, httpSessions, activeThreads, activeConnections, usedConnections, gc, threadCount, loadedClassesCount, usedNonHeapMemory, usedPhysicalMemorySize, usedSwapSpaceSize, httpSessionsMeanAge
  * and httpHitsRate, httpMeanTimes, httpSystemErrors (and same with sql, ejb, spring, guice, jsp ... instead of http)
  * and systemLoad, fileDescriptors for Linux and Unix
  * and tomcatBusyThreads, tomcatBytesReceived, tomcatBytesSent for Tomcat
  * and runningBuilds for Jenkins / Hudson nodes.

Note: The lastValue and jmxValue API may be used in a [Munin](http://munin.projects.linpro.no/) or [Nagios](http://www.nagios.org/) plugin, to make your own graphics or alerts using something like [this](https://github.com/coderholic/munin-popularity-plugins).

Note: There is also a [Nagios plugin](https://github.com/sbower/nagios_javamelody_plugin) by Shawn Bower based on a command reading the last values in the RRD files (it starts a JVM for each value, which may not scale a lot).

### jmxValue ###

For each MBean (JMX) attribute, a link on the left of the attribute in the MBeans page is provided to have only the current value of an attribute in plain text. For example, the following URL retrieves the bytes sent and the process cpu time for a Tomcat server: http://localhost:8080/mywebapp/monitoring?jmxValue=Catalina:type=GlobalRequestProcessor,name=http-8080.bytesSent|java.lang:type=OperatingSystem.ProcessCpuTime

To use the URL for a MBean value, the system actions must be [enabled](UserGuide#6._Optional_parameters.md) (true by default).


### XML ###

The monitoring data is available as XML over http if the [XStream and XPP3 dependencies](UserGuide#Dependencies.md) are available in the webapp. (XPP3 is a transitive dependency of XStream so you just need to add the XStream dependency if you have a Maven project).

**Warning**: The XML and JSON formats are dependent on the internal structure of data in JavaMelody. By using this XML/JSON format, you are coupled on this specific internal structure, which may change anytime without notice. (But of course, you may be more loosely coupled than by using the JavaMelody classes directly.) So use the XML/JSON API after acknowledging the risks of coupling.

The following URL retrieves data in XML format as a list of counters, with statistics of requests, and also some current system information:
http://localhost:8080/mywebapp/monitoring?format=xml&period=jour
You can also try with the public demo: http://demo.javamelody.cloudbees.net/monitoring?format=xml&period=jour

Like for the HMTL format, you can replace the "period" parameter with "semaine", "mois", "annee" and "tout" for the week, month, year and all periods. Or you can use a custom period.

Other data is available and the URLs are the same as for the HTML pages except that there is "format=xml&" added in the URL parameters.

For example, some aggregated statistics in a counter are available with the following URL:
http://localhost:8080/mywebapp/monitoring?format=xml&part=counterSummaryPerClass&counter=guice&period=jour

The "part" parameter in the URL can be replaced with "threads", "counterSummaryPerClass" and if system actions are [enabled](UserGuide#6._Optional_parameters.md) (true by default), it can be replaced with "heaphisto", "sessions", "mbeans", "jndi", "processes", "connections" or "database". ("counterSummaryPerClass" needs a "counter" parameter, "sessions" may need a "sessionId" parameter to have session details, "database" may need a "request" parameter to select a predefined database report and "jndi" may need a "path" parameter to select a jndi context path).

The response data is automatically compressed in gzip by JavaMelody, if the response size is more than 50 KB and if the client supports it (if the "Accept-Encoding" header contains "gzip" such as for browsers).


### JSON ###

The monitoring data is also available as JSON over http if the [XStream dependency](UserGuide#Dependencies.md) is available in the webapp. (the XPP3 transitive dependency is not needed for JSON).

The JSON API works the same as the XML API: just use "json" as "format" parameter instead of "xml" in the URLs given above.


### Serialized ###

The monitoring data is also available as java serialization over http, with "serialized" as "format" parameter.
This format is recommended only for the communication between a collect server and its monitored applications.


### API when using a collect server ###

Most of the API above works when using a [centralized collect server](UserGuideAdvanced#Optional_centralization_server_setup.md), including lastValue, jmxValue, XML and JSON.

You just need to add an "application" parameter with the name of the app, like "&application=myapp" in all the URLs above.

For example:
http://collectserver:8080/javamelody?part=lastValue&graph=usedMemory,cpu&application=myapp

or http://collectserver:8080/javamelody?format=xml&period=jour&application=myapp


When monitoring an application with several monitored instances (cluster), some URLs retrieve aggregated data and some other URLs retrieve unaggregated data from a collect server. For example, the lastValue API and the statistics/heaphisto/sessions in XML format retrieve aggregated data, but the jmxValue API and the threads/processes in XML format retrieve unaggregated data: try the URLs by yourself.