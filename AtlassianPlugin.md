# Atlassian JIRA, Confluence and Bamboo Plugin #

**Attention**: The plugin managers may say that the latest javamelody versions are incompatible, but in fact they are certainly compatible as displayed on the [plugin history page](https://marketplace.atlassian.com/plugins/net.bull.javamelody/versions).

> If you want to monitor a [JIRA](http://www.atlassian.com/software/jira/),
> [Confluence](http://www.atlassian.com/software/confluence/) or a [Bamboo](http://www.atlassian.com/software/bamboo/) server, you can install JavaMelody easily
> with a JIRA/Confluence/Bamboo plugin. For this, [download](https://github.com/javamelody/javamelody/releases) the jira-javamelody.jar file of the plugin
> and copy the file:
    * in the "`JIRA x.x.x/atlassian-jira/WEB-INF/lib`" directory for JIRA,
    * in "`Bamboo/atlassian-bamboo/WEB-INF/lib`" for Bamboo (or in "`Bamboo/webapp/WEB-INF/lib`" for older Bamboo)
    * or upload the file with the "Plugin manager" in the "Administration" menu for Confluence.

> Restart the server.

> Then you can open the report from the address `http://<host>:<port>/monitoring` (the "Monitoring" link is also available in the "Administration" menu).
> That's it in general.

> Notes: The jdbc connections and the SQL requests are monitored automatically in JIRA 4.3.x or before, and they are monitored in JIRA 4.4+, Confluence and Bamboo if a jdbc datasource from jndi is used, instead of the default direct jdbc connection.

> To configure Confluence with a datasource, see [this](http://confluence.atlassian.com/display/DOC/Database+Configuration) or [this](http://confluence.atlassian.com/display/DOC/Configuring+a+PostgreSQL+Datasource+in+Apache+Tomcat).

> To configure Bamboo with a datasource, see [this](http://confluence.atlassian.com/display/BAMBOO/Connecting+Bamboo+to+an+external+database).
> Alternatively for Bamboo, it is possible to replace the default hsqldb driver by the following in `bamboo-home/bamboo.cfg.xml`:
```
	<property name="hibernate.connection.driver">org.hsqldb.jdbcDriver</property>
	<property name="hibernate.connection.driver_class">net.bull.javamelody.JdbcDriver</property>
```

> Since **JIRA 4.4**, the database configuration has been changed by Atlassian as said [here](http://confluence.atlassian.com/display/JIRA/JIRA+4.4+Upgrade+Notes#JIRA44UpgradeNotes-MigratingYourDatabaseConfiguration).
> If you are using JIRA 4.4 or later, you need to use a jdbc datasource from jndi in order to monitor jdbc connections and SQL requests.

> To use a datasource, replace in the dbconfig.xml file "`<jdbc-datasource>...</jdbc-datasource>`"
> with "`<jndi-datasource><jndi-name>java:comp/env/jdbc/JiraDS</jndi-name></jndi-datasource>`" (take a backup before),
> and add a Resource element for a datasource inside the Context element of the Tomcat's "JIRA/conf/server.xml" file:

```
	...
	<Context ... > 
		...
		<Resource name="jdbc/JiraDS" auth="Container" type="javax.sql.DataSource"
			username="[enter db username]"
			password="[enter db password]"
			driverClassName="com.mysql.jdbc.Driver"
			url="jdbc:mysql://localhost/jiradb?useUnicode=true&amp;characterEncoding=UTF8"
			maxActive="20"
			maxIdle="10"
			validationQuery="select 1"
		/>
       </Context>
	...
```

> Or see this [example of datasource from Atlassian](http://confluence.atlassian.com/display/JIRA043/Connecting+JIRA+to+MySQL#ConnectingJIRAtoMySQL-51ConfigureyourapplicationservertoconnecttoMySQL). The values in your Resource element should be inspired by the values that were just removed from your dbconfig.xml file.
> And so the jndi datasource will be automatically monitored after restart.


> The data files will be stored by default in <installation directory>/temp/javamelody/. If you want to use another **storage directory**, add a "javamelody.storage-directory" system property (in <installation directory>/bin/setenv.sh or in <installation directory>/conf/catalina.properties). For example:

```
	... -Djavamelody.storage-directory=/my/path
```

> In JIRA, Confluence and Bamboo, you can also send **weekly, daily or monthly reports** in pdf format by mail to one or several people.
> This is explained in the chapter [Weekly, daily or monthly reports by mail](UserGuide#14._Weekly,_daily_or_monthly_reports_by_mail.md).

> For example, for JIRA:

> Copy the "mail-1.4.4.jar" and "activation-1.1.1.jar" files from the "JIRA/atlassian-jira/WEB-INF/lib/" directory
> to the "JIRA/lib/" directory.
> And add the following Resource and Parameter lines inside the Context element of the "JIRA/conf/server.xml" file:

```
	...
	<Context ... > 
		...
		<Resource name="mail/JavaMelodySession" auth="Container" type="javax.mail.Session"
			mail.smtp.host="mailhost.foo.bar"
			mail.smtp.user="someLoginIfNeeded"
			mail.from="javamelody@foo.bar"
		/>
		<Parameter name='javamelody.admin-emails' value='admin1@company.com,admin2@company.com' override='false'/>
		<Parameter name='javamelody.mail-session' value='java:comp/env/mail/JavaMelodySession' override='false'/>
		<Parameter name='javamelody.mail-periods' value='day,week,month' override='false'/>
	</Context>
	...
```

> Read [Weekly, daily or monthly reports by mail](UserGuide#14._Weekly,_daily_or_monthly_reports_by_mail.md) for more parameters if needed.