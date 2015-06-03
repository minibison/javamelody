## Continuous Integration of JavaMelody ##

[Continuous integration](http://en.wikipedia.org/wiki/Continuous_integration) can be used to continuously make nightly builds of JavaMelody and to make reports like unit tests, javadoc or browsable sources.

### Built on CloudBees ###

<a href='http://www.cloudbees.com/'><img src='http://javamelody.googlecode.com/svn/trunk/javamelody-core/src/site/resources/images/Button-Built-on-CB-1.png' alt='Built on CloudBees' width='20%' /></a>
There is now a public Jenkins Continuous Integration server hosted by [CloudBees](http://www.cloudbees.com) for the JavaMelody project:

You can download the [nightly build](https://javamelody.ci.cloudbees.com/job/javamelody/) (snapshot from trunk).

You can also browse the latest [javadoc](https://javamelody.ci.cloudbees.com/job/javamelody/site/apidocs/index.html), [sources](https://javamelody.ci.cloudbees.com/job/javamelody/site/xref/index.html) and other [reports](https://javamelody.ci.cloudbees.com/job/javamelody/).

There is also a [continuous integration](https://javamelody.ci.cloudbees.com/job/jenkins%20plugin/) for the [Jenkins Monitoring plugin](https://wiki.jenkins-ci.org/display/JENKINS/Monitoring).

### Private Continuous Integration ###

If you have a private CI server, you can easily configure it to make nightly builds and reports.

The following explains how to do this with [Jenkins](http://jenkins-ci.org/) which is a very good continuous integration server.

  * First, if you don't have Jenkins, get it from http://jenkins-ci.org/ and be sure to have [maven 3.0.4](http://maven.apache.org) or later configured in "Manage Jenkins / Configure system".
  * After installation, open the Jenkins server page in your browser and click on the _New job_ link.
  * Write _JavaMelody_ in "job name", select _Build a maven2 project_ and click "OK".
  * In "Source Code Management" of the configuration page, select _Subversion_.
  * And in "Repository URL", write _https://javamelody.googlecode.com/svn/trunk/javamelody-core_
  * In "Build Triggers", check _Poll SCM_ and write _@daily_ or _@weekly_ in the "Schedule" area.
  * In "Build", write "_clean install site -e_" in the "Goals and options" area.
  * In "Build" also, click the "Advanced..." button and write "_-Xmx128m_" in "MAVEN\_OPTS".

  * If you already have the Jenkins plugins checktyle, findbus, pmd, dry and corbertura installed in your server, then you can check all of them in "Build settings" to generate charts and reports.
  * Click "Save" at the bottom to save the configuration
  * And click "Schedule a build" to run the job. (Note: the first time, wait for maven to download some dependencies).

  * When the job is finished, you can open the junit reports from the Jenkins job page or you can open the "Maven-generated site".
  * In the maven site, you can click on the green arrow to download the latest zip of project including the javamelody.jar, or you can open the generated user guide or the project reports (checkstyle, findbugs...) from the menu.

And of course, you can also install the "[Monitoring](http://wiki.jenkins-ci.org/display/Jenkins/Monitoring)" Jenkins plugin in your Jenkins server to monitor it with JavaMelody, or if you use [JIRA](http://www.atlassian.com/software/jira/) you can install the "[Monitoring](https://plugins.atlassian.com/plugins/net.bull.javamelody)" JIRA plugin.

In Jenkins, the [violations](http://wiki.jenkins-ci.org/display/JENKINS/Violations), [cobertura](http://wiki.jenkins-ci.org/display/JENKINS/Cobertura+Plugin) and [tmp cleaner](http://wiki.jenkins-ci.org/display/JENKINS/Tmp+Cleaner+Plugin) plugins can also be recommended.