# Spark Jetty Server

This is a simple example demonstrating how to embed a ```SparkContext``` within a Jetty web server running in yarn-client mode.
This proved to be non-trivial as an understanding of how the Spark classpath is built is quite necessary to make this work. So
far, this has only been tested using the Jetty maven plugin, but it should translate fairly easily to an actual jetty instance.

Originally forked from [calrissian/spark-jetty-server](https://github.com/calrissian/spark-jetty-server), this no more needs any classpath setup like ```SPARK_HOME```, ```HADOOP_HOME``` etc. I've modified the original project to build a fat jar using maven shaded plugin in order to include all needed dependencies in the final jar.

Hence, running this project is now much more simplified without the need for the user to worry about setup's. Just download and run as shown below.

## Building and running

The default build works with Spark 2.1.0 and Hadoop 2.7.0 but the versions are supplied through maven properties. To use a different Spark version, add ```-Dspark.version=<newVersion>``` to the maven command. To use a different Hadoop version, add ```-Dhadoop.version=<newVersion>``` to the maven command.

1. Build the project from the root using ```mvn clean install```.

2. ```cd``` into the ```war/``` directory and run ```mvn jetty:run```.

3. Once the webserver is up and running, navigate to ```http://localhost:8080/test``` and watch the result of a quick job.


## Other thoughts

Perhaps the Spark documentation could better help users understand what's really going on behind the scenes with the classpaths of the various components involved (executor, master, driver, etc...). In this example, a SparkContext is instantiated by Spring's ApplicationContext at the deployment of the web application. This negotiates some resources ahead of time so that jobs can be run quickly without the overhead of having to negotiate those resources and fire up JVMs each time a job needs to be run. If this is not desired, it would be easy enough to have several different SparkContexts that run within the various different web application scopes (request, session, etc...). 

Hopefully this will open up new doors for implementing different real-time query and data manipulation use-cases for Spark.
