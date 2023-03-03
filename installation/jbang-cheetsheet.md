Here is **JBang cheatsheet** 

## Getting started with JBang

Install JBang on Linux

curl -Ls https://sh.jbang.dev | bash -s - app setup

curl -Ls https://sh.jbang.dev | bash -s - app setup

```
curl -Ls https://sh.jbang.dev | bash -s - app setup
```

Run a JBang Script

```
jbang example.java
```

Create an executable JAR from a JBang script:


```
jbang export portable MyApp.java
```

Create a JBang class  from a template

```
jbang init --template=hello helloworld.java
```

List of available JBang templates

```
jbang template list
```

Run a Java class

```
jbang helloworld.java
```

How to run a Java class in a JAR file


```
jbang jgroups-4.2.15.Final.jar --main=org.jgroups.tests.McastReceiverTest
```

How to run a Java class using Maven GAV coordinates

```
jbang org.jgroups:jgroups:5.1.9.Final --main=org.jgroups.tests.McastReceiverTest 

```

How to run a Java code snippet:

```
echo "hello world" | jbang -c 'lines().forEach(s->println(s.substring(6)))`
```

## Update JBang

To update JBang run the following command:

```
jbang version --update
```

Please note that if your current JDK is not aligned with the latest version of Java installed with JBang (see section JDK Configuration), the following error can occur during the update:

```
javax.net.ssl.SSLHandshakeException: Received fatal alert: handshake_failure
```

## Configuration

Adding dependency

```
//DEPS log4j:log4j:1.2.17
```

Adding GitHub project (JBang will download the project, build it as use it as a dependency)

```
//DEPS https://github.com/DiUS/java-faker
```

Adding Managed Dependencies (BOM)

```
//DEPS io.quarkus:quarkus-bom:1.11.0.Final@pom
//DEPS io.quarkus:quarkus-resteasy
//DEPS io.quarkus:quarkus-smallrye-openapi
```

Using System properties in the comments

```
//DEPS org.openjfx:javafx-graphics:11.0.2:${os.detected.jfxname}
```

Using Environment variables in the comments

```
//DEPS org.openjfx:javafx-graphics:11.0.2:${env.jfxname}
```

Repository for dependencies (maven repository + custom repository)

```
//REPOS mavencentral,acme=https://maven.acme.local/maven
```

Finally, please note that you can also pass the dependencies as command line argument:

```
jbang --deps org.matheclipse:matheclipse-core:2.0.0,org.matheclipse:matheclipse-io:2.0.0 test.java
```

Adding file resources

```
//FILES resource.properties
//FILES META-INF/resources/index.html=index.html
```

Adding a Manifest file:

```
//MANIFEST version=${version:unknown}
```

Setting Quarkus properties

```
//Q:CONFIG quarkus.datasource.db-kind=postgresql
```

Remote file expansion (JBang will download the remote file – tagged with % – and pass it as argument)


```
jbang CountWords.java %https://github.com/dwyl/english-words/raw/master/words.txt
```

## Java Sources

Location of Java Sources

```
//SOURCES nested/*.java
//SOURCES othernested/*.java
```

You can also pass the sources (and resources) location from the command line:


```
jbang --sources src/main/java --files src/main/resources script.java
```

## JDK configuration

Setting JDK in your code

```
//JAVA 11 will force use of Java 11.
//JAVA 13+ will require at least java 13. Java 13 or higher will be used.
```

Install a JDK

```
jbang jdk install 17
```

Install a JDK already available on your machine:

```
jbang jdk install 17 `sdk home java 17.0.4.1-tem`
```

Uninstall a JDK

```
jbang jdk uninstall 14
```

List available JDK

```
jbang jdk list
```

Setting default JDK

```
jbang jdk default 12
```

## Debugging and JDK Settings

Debugging a Java class

```
jbang --debug helloworld.java
```

Setting java and javac options

```
//JAVAC_OPTIONS -source 11
//JAVA_OPTIONS -Xms128m -Xmx512m
```

## Catalog

Install an app from the Catalog

```
jbang app install CamelJBang@apache/camel
```

List of available apps

```
jbang app list
```
