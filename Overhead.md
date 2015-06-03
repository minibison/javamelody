# Overhead of JavaMelody #

Overhead of monitoring tools is often a question, for example in a production environment.

There were discussions about JavaMelody overhead [here](https://groups.google.com/forum/#!msg/javamelody/39txASF__Lw/acjcucbmxlcJ) and [here](https://groups.google.com/forum/#!topic/javamelody/Em5L-ScNaxo/overview). I paste them below for archive.

### evernat (Nov 25, 2009) : ###

Architecture of JavaMelody is lightweight. So it has a lower overhead as I can compare it to other available solutions. The overhead is so low that it can be enabled continuously in QA environment. And if no problem arises in QA, also continuously in production environment : it is made for that. (It is already used in QA and production with an application of 25 person years.)

Why is the overhead low ?
  * First, it does monitoring not profiling : there is no instrumentation of classes like some well known profilers (NetBeans profiler for example) or some monitoring tools. JavaMelody has instead "interceptors" for http, jdbc by default, and spring or ejb3 if you wish so.
  * Second, I would say that the overhead of some other good monitoring solutions is in the recording of each event in a database or in a master server. With JavaMelody, there is no database and no recording of each events even in a file or over the wire like most others : only statistics of requests are kept, so the overhead of cpu is minimal with no I/O on the wire and minimal I/O on disk (just to take a backup of statistics at a regular interval). Likewise, it is only statistics and not events so the overhead of memory is quite minimal.
  * Third, with little overhead you will be able to know what needs optimizing in the QA or production server(s). So that the overhead of JavaMelody will be negative.
  * Fourth, if overhead does really some pain for your app with thousands active users, you will have the choice to use javamelody centralized collect server. It unloads the memory, the backup storage and the generation of reports to another server while adding I/O on the wire for sending deltas of the statistics.
  * Fifth, the financial overhead is zero as JavaMelody is opensource and free.

For full disclosure, some system actions can consume some cpu. These system actions are : Launch the garbage collector, Take a heap dump.

I do not have figures, but overhead is probably under 5% of cpu and of memory, except if you are not reasonable at all when enabling monitoring of spring or ejb3.


### dkarlsen (May 23, 2011) : ###

[Roots conference slides](http://fr.slideshare.net/djkarlsen/significance-of-metrics) on using JavaMelody in production (slide 3).

### Dicussion (Dec 4, 2012) ###

**iamravi**

Hi, Can you please let me know the memory footprint of using this tool in PROD. Can we see how much memory is used by jmelody in general. Also is it possible to switch on/off this tool in PROD. When any issue happens we want to switch it ON and then OFF without restarting the server.

Thanks, Ravi

**zhenek**

I don't think it is good idea to switch off javamelody as most useful are historical data measured before you run into any problem and how would you know without monitoring tool there is problem? How whould you know if running 500 threads in your application is causing problem without knowing that common number of threads is 200, etc.

JavaMelody is to know about problems before your users find out that your application has problem. We use JavaMelody with Nagios and get sms report everytime something may be wrong soon (warnings) or there is already problem (Critical reports).

I have never measured memory consumption exactly, but our application started in tomcat has around 200Mb of ram when started with or without JavaMelody then I would say memory consumption is not high. You could measure yourself using jvisualvm if you need exact numbers.

**dhartford**

I also regularly run javamelody **instead** of most other java-specific monitoring tools around applications because it is low memory/low overhead.  Again, please measure yourself.

Just as an FYI, javamelody is a statistic collector, making it quite efficient, opposed to trace or reflection based tools. The downside is you only get statistics on what is **used**, rather than a profile of your entire application, which for some people usage-based is far more useful! :-)

**evernat**

I will tell a story, living it myself for about 2 weeks.

My client for those 2 weeks is a public authority. It has problems to run its financial application and to manage its budgets and expenses.
This application has 11 tomcat instances in production for about 300 users. Because it is the end of the year, each user of the application is trying to find budgets everywhere for expenses, and "of course" they are sure that available budgets will be reset to 0 on the first day of January.

JavaMelody monitoring was installed in test, with no problem. Then, it was installed in production with no problem.
3 days later, there was problems again in production and the big chief decides to disable the monitoring in production, because "the production servers have problems and the monitoring tools have a big overhead in general".

But, it was a fact given by the monitoring, that the tomcat servers were run in jvm's client mode instead of server mode (wow !), and that some SQL requests took more than 10 minutes to execute.
Those are critical problems, but sadly they are quite common for financial applications in my experience.


I will remember two things: the JavaMelody overhead in production was zero in this case again, unlike some other monitoring tools, and the IT people certainly need basic visibility of the applications if they need to do some work.
Anyway, JavaMelody monitoring for 3 days was enough to me to know that the application will die several times before the New Year. Users will wait, again.

**dhartford**

I can add one more story, these would be good to keep somewhere on the public site.

Although not 'production', I went through the effort of using a specific (fast, no issue) scenario to really see the performance impact...as far as delay/latency rather than memory/resources.

Pushing 50,000 queries through with javamelody jdbc tracking enabled, the averaged deltas over 5 runs of 50k was each query had an additional 0.6ms cost.  6/10th of a millisecond.  Some people may take that at face value, but when you are averaging 5-15ms per query w/ jdbc, connection pooling, transaction management....the 6/10th is a really, really low price to pay for when an issue does become visible...especially as your applications grow in features and complexity.

Javamelody being a usage-based statistic collector is a really good direction for developer-time productivity, server-resource efficiency, real-world based review of your applications to know where your real-world issues exist rather than relying on pre-optimization measure/improvements.  I use it in user-acceptance testing (users testing app as they would normally), integration testing for services, and production to identify 'top' issue areas to focus on rather than educated guesses/assumptions.

**iamravi**

Thanks everyone for the reply and making it clear that usage of JavaMelody will not result into memory footprint.
One more clarification needed from experts out there.
We have used JavaMelody on our DEV, stage and performance test boxes but is it a good practice to switch on JavaMelody in PROD when there are no issues. We want to use it in PROD only when we see any issues otherwise not. Is there a way of achieving the same without server restart?

**dkarlsen**

I would keep it on in production normally. So that you know what the normal figures are for comparison when anything breaks.  And then you Don't need to turn around anything when in trouble.

**zhenek**

Yes I would recommend running javamelody in production.
  1. you get stats and you know what is unusual
  1. production doesn't use test data, but sharp user data which could cause problems which need to be detected
  1. JavaMelody is able to generate pdf reports and you could get weekly emails with these reports

Imagine this situation:
Your application is unstable, but you don't know why. There are 500 threads running, app takes 3gb of ram and there is opened 15000 filedescriptors.

All values seems high, but all may be ok if your application was working fine till now with same values.

With JavaMelody you could find out historical data and detect what has changed when your application is unstable. You could quickly focus on problems related to statistics. If you know that all these (500 threads running, app takes 3gb of ram and there is opened 1500 filedescriptors) are common you know you have
to look somewhere else. If you know your application usually takes 500 mb then the problem is memory and you could check application logs to see what your
application logged when the memory went high.

Currently we have JavaMelody embedded to all our applications and JavaMelody is automatically up everytime and mostly used in production and helps solving unexpected problems.