# DevOps Challenge 3 - Wildfly Java Application Deployment

## Time for completing module 4 hours

## References
* https://docs.jboss.org/author/display/WFLY8/CLI+Recipes


## Objective
In this challenge you should take the soruce code from the repository and have Jenkins compile and build the artifact.  This artifact should then be available for deployment to a Wildfly server which has a connection to your MySQL database server.

You should deploy the application to your Wildfly server, and perform the necessary checks to see if you need to configure the database for the first time, or whether it's a redeployment of the application.  If the database is not configured or the JDBC driver not installed this should be performed.

You should release your Wildfly application and server as a Docker image that can be deployed from your Docker registry.

## Before automation
You will need to do a manual installation of the application first to see all of the tasks required to build and deploy the application, since you will need to discover how to add database configurations to Wildfly XML or through the command line.

You will need to create the database

You will need to ensure that the correct JDBC driver is installed on your Wildfly server, and that the Datasource is set correctly.  It is possible to place the JDBC driver in the **deployment** directory if you are unable to install it through yum or apt-get.

Careful with Java as certain versions will cause problems, and not Just Java, but libraries too.

### Connecting to JBoss using the CLI;
```
/opt/jboss/wildfly/bin/jboss-cli.sh --connect controller=localhost
```

Running non-interactively:
```
/opt/jboss7/bin/jboss-cli.sh --connect --command='ls /host=nodeName/server-config=serverName' | grep 'status='
```

### Configuring the DB connection in Wildfly through CLI;
```
/subsystem=datasources/data-source=MySQLDatasource: \
   add( \
   jndi-name="java:/mySQL", \
   driver-name="mysql-connector-java-5.1.32-bin.jar_com.mysql.jdbc.Driver_5_1", \
   connection-url="jdbc:mysql://172.17.0.5:3306/conygre", \
   user-name="root", \
   password="secret")
```

Note you will need to specify;
* your MySQL database IP address or name
* The version and name of the mysql-connetor being used
* The username and password used to connect to your database

A successful install and connection to MySQL with the correct JDBC driver would be;
```
[standalone@localhost:9990 /] /subsystem=datasources/data-source=MySQLDatasource: \
>    add( \
>    jndi-name="java:/mySQL", \
>    driver-name="mysql-connector-java-5.1.32-bin.jar_com.mysql.jdbc.Driver_5_1", \
>    connection-url="jdbc:mysql://172.17.0.5:3306/conygre", \
>    user-name="root", \
>    password="secret")
{"outcome" => "success"}
```

**NOTE:** The **outcome** showing **success** is what we want.

If there is an issue with the database or the JDBC driver you will see;
```
[standalone@localhost:9990 /] /subsystem=datasources/data-source=MySQLDatasource: \
>    add( \
>    jndi-name="java:/mySQL", \
>    driver-name="mysql-connector-java-5.1.32-bin.jar_com.mysql.jdbc.Driver_5_1", \
>    connection-url="jdbc:mysql://172.17.0.5:3306/conygre", \
>    user-name="root", \
>    password="secret")
{
    "outcome" => "failed",
    "failure-description" => {
        "WFLYCTL0412: Required services that are not installed:" => ["jboss.jdbc-driver.mysql-connector-java-5_1_32-bin_jar_com_mysql_jdbc_Driver_5_1"],
        "WFLYCTL0180: Services with missing/unavailable dependencies" => [
            "org.wildfly.data-source.MySQLDatasource is missing [jboss.jdbc-driver.mysql-connector-java-5_1_32-bin_jar_com_mysql_jdbc_Driver_5_1]",
            "jboss.driver-demander.java:/mySQL is missing [jboss.jdbc-driver.mysql-connector-java-5_1_32-bin_jar_com_mysql_jdbc_Driver_5_1]"
        ]
    },
    "rolled-back" => true
}
```

**NOTE:** The **rolled-back** being set to true tells you that Wildfly did not take the update.

#### After the CLI
If the CLI command is successful the **standalone.xml** file is updated to include your configuration.

## On completion
If the application is successfully deployed you should be able to point your web browser at;
* http://localhost:1080/CompactDiscRESTEnterprise-0.0.1-SNAPSHOT/rest/compactdiscs

This should return some JSON output;
```
{"discCollection":[{"id":9,"title":"Is This It","artist":"The Strokes","price":13.99,"tracks":11},{"id":10,"title":"Just Enough Education to Perform","artist":"Stereophonics","price":10.99,"tracks":11},{"id":11,"title":"Parachutes","artist":"Coldplay","price":11.99,"tracks":10},{"id":12,"title":"White Ladder","artist":"David Gray","price":9.99,"tracks":10},{"id":13,"title":"Greatest Hits","artist":"Penelope","price":14.99,"tracks":14},{"id":14,"title":"Echo Park","artist":"Feeder","price":13.99,"tracks":12},{"id":15,"title":"Mezzanine","artist":"Massive Attack","price":12.99,"tracks":11},{"id":16,"title":"Spice World","artist":"Spice Girls","price":4.99,"tracks":11}]}
```
