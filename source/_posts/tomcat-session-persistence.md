---
title: Tomcat Session Persistence
date: 2019-06-28 21:47:01
tags:
---
Session persistence is an interesting feature in Apache Tomcat. Using this we can potentially have multiple instances of tomcat running on different physical machines and they can share the sessions across the running instances, potentially providing capability for a user's request to be juggled between different instances of Tomcat yet maintaining session dependent contextuality.
Let's see how we can do this using Postgres DB as a store of sessions. We need a table where the different instances of Tomcat can store and synchronize session related information.

```
CREATE TABLE tomcat_sessions
(
  session_id character varying(100) NOT NULL,
  valid_session character(1) NOT NULL,
  max_inactive integer NOT NULL,
  last_access bigint NOT NULL,
  app_name character varying(255),
  session_data bytea,
  CONSTRAINT tomcat_sessions_pkey PRIMARY KEY (session_id)
);
```
Because we want this to be accessed fast by the name of application let's index it. 

```
CREATE INDEX app_name_index
  ON tomcat_sessions
  USING btree
  (app_name);
```

Okay so now we have a place where we can store the session related information, let's tell our tomcat instances about this. 
Find the `$tomcat-root/conf/context.xml`, the `Manager` is by default commented out so that tomcat does the persistence of sessions on disk by default. Add this:

```
<Manager className="org.apache.catalina.session.PersistentManager" processExpiresFrequency="3" maxIdleBackup="1" >
        <Store className="org.apache.catalina.session.JDBCStore"
        	driverName="org.postgresql.Driver"
	        connectionURL="jdbc:postgresql://localhost:5432/websessions"
        	connectionName="postgres" connectionPassword="root"
	        sessionAppCol="app_name" sessionDataCol="session_data" sessionIdCol="session_id"
        	sessionLastAccessedCol="last_access" sessionMaxInactiveCol="max_inactive"
		sessionTable="tomcat_sessions" sessionValidCol="valid_session" />
</Manager>
```

Now because we are using PostgreSQL we need to add the JDBC libary to the `$tomcat-root/lib` folder.
Once we do that we are done.
[cluster-manager](https://tomcat.apache.org/tomcat-9.0-doc/config/cluster-manager.html)
[manager](https://tomcat.apache.org/tomcat-9.0-doc/config/manager.html)
[manager-howto](https://tomcat.apache.org/tomcat-9.0-doc/manager-howto.html)
[cluster-howto](https://tomcat.apache.org/tomcat-9.0-doc/cluster-howto.html)
[context](http://localhost:8080/docs/config/context.html)
