# MAVEN
Standalone java app: JAR file  
Web app: WAR file  
But as we know is we use thymeleaf, it does use JAR file, even if it is a web application. 
It uses HTML, and all the tags Spring internally handle.

Archetypes: like as template. Stand-alone, webapp, ear etc

Build, dependency management tool

PLugins:
a. compiler plugin  
b. surefire plugin for unit test  
c. wsimport: generate Stub from a web services   

Set maven: M2_HOME=PATH_TO_MAVEN  
add %PATH_TO_MAVEN%/bin to "path"  

config/settings.xml: config file where we can customize the behaviour of maven

we will need to place settings.xml in .m2 (which maven create automatically)


Create a have project from maven:
```
mvn archetype:generate -DgroupId=com.abc -DartifactId=hello-maven -DarchetypeArtifactId=naven-archetype-quickstart -DinteractiveMode=false
```


mvn clean insall   
and then to run   
java -jer <jar>  
or java -jer <jar>  com.anc.className  


Plugins and Goals: A maven plugin is a collection of one or more goals. clean, install etc are the goals.

Below is generate goal from archetype plugin
```
mvn archetype:generate -DgroupId=com.abc -DartifactId=hello-maven -DarchetypeArtifactId=naven-archetype-quickstart -DinteractiveMode=false
```
install goal from install plugin

BY default maven does not know, how to create project, package it etc... it uses "compiler" etc to get the work done. Every maven project gets a default set 
of plugins from its parent., but we can override then by defining them in our pom file.



 



Surefire Plugin:
We can run the tests of a project using the surefire plugin. By default, this plugin generates XML reports in the directory target/surefire-reports.
This plugin has only one goal, test. This goal is bound to the test phase of the default build lifecycle, and the command mvn test will execute it.
The surefire plugin can work with the test frameworks JUnit and TestNG. No matter which framework we use, the behavior of surefire is the same.
By default, surefire automatically includes all test classes whose name starts with Test, or ends with Test, Tests or TestCase.
We can change this configuration using the excludes and includes parameters, however:

```
<plugin>
    <artifactId>maven-surefire-plugin</artifactId>
    <version>2.22.2</version>
    <configuration>
        <excludes>
            <exclude>DataTest.java</exclude>
        </excludes>
        <includes>
            <include>DataCheck.java</include>
        </includes>
    </configuration>
</plugin>
```
With this configuration, test cases in the DataCheck class are executed while the ones in DataTest aren't.


Maven life cycle phases:
1. process-resources 
2. compile
3. test
4. package etc

if we run once of them, will execute all previous phases incl. requested phases. If we write package, will run process-resources, compile, test
Each phases are associated with 1 or more goals. there can be multiple goals associated with plugins, but generally there is 1 goal associated.

    Phase                       Plugin:Goals
    ----                        -----------------
    process-resoutce            resources:resources
    compile                     compiler:compiler
    test                        surefire:test
    package                     jar:jar
    
    
mvn test === mvn surefire:test
    
If it is standalone java project, maven will be associated with jar:jar; if it is web project then maven dunamically associated with war:war goal


Maven Lifecycle:
clean
validate
compile
test
package
verify
install
site
deploy



groupId:artifactId:packaging:version: uniquely identified where we can find an artifact in maven repository
groupId: java package, it is the reverse the url of company like org.oracle. Cna incl sub package
artifactId; artifact name (this will be the jar name ete)
version
packaging: jar/war


default mavn repository: http://repo.maven.apache.org/maven2
This is the location maven build is output the location for me
If the jar is nt available is central repository (http://repo.maven.apache.org/maven2), we can look for enterprise level repository (org level repository)
Maven create a local repository on developers m/c so everytime it will not pull them from central ir enterprise repo. This is under .me (user directory/.m2)

every pom is derived for  super pom. This as well. (pom.xml -> right click -> effective pom)

```

<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.example</groupId>
    <artifactId>MavenTutorials</artifactId>
    <version>1.0-SNAPSHOT</version>
    <name>hellomaven</name>
    <url>http://www.zbcxzxzcxcxc.co</url>

    <properties>
        <maven.compiler.source>1.8</maven.compiler.source> <!-- source code compiler -->
        <maven.compiler.source>1.8</maven.compiler.source> <!--oldest jew version you need to be supported -->
    </properties>

</project>

```

Say in effective pom the compiler vrsion is old, and we want to use newer version. Add below:

```
 <plugin>
        <artifactId>maven-compiler-plugin</artifactId>
        <version>3.1</version>
        <executions>
          <execution>
            <id>default-compile</id>
            <phase>compile</phase>
            <goals>
              <goal>compile</goal>
            </goals>
          </execution>
          <execution>
            <id>default-testCompile</id>
            <phase>test-compile</phase>
            <goals>
              <goal>testCompile</goal>
            </goals>
          </execution>
        </executions>
      </plugin>

```

skipping test:
```
mvn clean install -DskipTests
```

Below will limit the dependency, to the test; and will not add it while source code is compiled
<scope>test</scope>

provided scope: generally for web application.This means it will not be added while creating war etc... this will be there for compile time when needed but during 
runtime, this will not be included as tomcat etc already have this dependency.


Add parent child project

parent pom:
```
    <groupId>org.example</groupId>
    <artifactId>MavenTutorials</artifactId>
    <version>1.0-SNAPSHOT</version>
    <name>hellomaven</name>
    <url>http://www.zbcxzxzcxcxc.co</url>

    <modules>
        <module>one</module>
        <module>two</module>
    </modules>

```


child pom:
```
    <parent>
        <artifactId></artifactId>
        <artifactId></artifactId>
        <version></version>
    </parent>
```

in multi module project, maven reactor decides the project/module's build sequence. If a module (module2) denepds on module1, them module will be built first and then module 2

If endpoint is giving 404:
1. check pom and make sure we are using latest spring
2. reimport dependency
3. mvn clean
4. mvn install
5. then run

delete repository from .m2

My understanding: If we want to do something extra with maven goal we will need to add required plugin. Say we want a report generated (not surefire report) while run 'mvn install'.


Scopes:
1. compile:  Default option. Avl during project build, test run, app un.
2. provided: Req. during build, test and run and not req. to be exported. i.e. when deployed not needed as tomcat ect already has that dependency.
3. runtime: Runtime of test and application. Not available during compile time.  
4. test: compile and run the test. like junit.
5. system: Less use. Similar as provided but not tomcat etc providing. need to use system path in sub-directory.   
    ```
    <systemPath> 
            ${basedir}\war\WEB-INF\lib\extDependency.jar
   ```
6. import: used for pom based, not for jar or war 


In parent pom, if we add this, it will actually denotes, we will not needed to add the <version> for junit whereever we add it in parent and child pom
So, this will ensure, we are using same junit version throughout all the parent and child projects. 
Still if we define the junit version in child amd/or parent project under <dependencies>, it will override the managed version which is mentioned 
under <dependencyManagement>

```
<dependencyManagement>
    <dependency>
        <groupId>org.junit.jupiter</groupId>
        <artifactId>junit-jupiter-api</artifactId>
        <version>5.8.2</version>
     </dependency>
</dependencyManagement>

```


similar to the <dependencyManagement>, we have option for plugin management. This help us with the consistency of the plugin we used.


Profiles:


/dev/application.properties
/test/application.properties
/qa/application.properties
/prod/application.properties

```
    <profile>
        <id>dev</id>
        <properties>
            <build.profile.id>dev</build.profile.id>
        </properties>
        <build>
            <resources>
                <resource>
                    <directory>src/main/profiles/dev</directory>
                </resource>
            </resources>
        </build>
    </profile>

```

mvn install -p dev //-p is profile and activating dev profile

so only dev file and details will be added in JAR

jcoco code coverage:

1. Need to add jcoco plugin in pom

```

    <plugin>
        <groupId>org.jacoco</groupId>
        <artifactId>jacoco-maven-plugin</artifactId>
        <version>0.8.7</version>
    </plugin>

```
2. Initialize the jcoco plugin
3. set up the goal
```
    <plugin>
        <groupId>org.jacoco</groupId>
        <artifactId>jacoco-maven-plugin</artifactId>
        <version>0.8.7</version>
        <executions>
            <!-- Initialize the jcoco plugin -->
            <execution>
                <goals>
                    <goal>prepare-agent</goal>
                </goals>
            </execution>
            <!-- means mvn test will kick off this 'report' goal -->
            <execution>
                <id>report</id>
                <phase>test</phase>
                <goals>
                    <goal>report</goal>
                </goals>
            </execution>
        </executions>
    </plugin>

```
3. Run the goal

mvn clean test
mvn clean verify //verify runs and check the result of test  

report will be there under target/site/jcoco


Sonarqube is static code analysis tool



1. Install sonar 
    a. download from internet (free edition)
    b. unzip
    c. run StartSonar.bat
    d. http://localhost:9000/ (admin/admin) [i changes it to admin/password]
    
2. RUn sonar locally
3. add settings.xml under .m2
```
<settings>
    <pluginGroups>
        <pluginGroup>org.sonarsource.scanner.maven</pluginGroup>
    </pluginGroups>
    <profiles>
        <profile>
            <id>sonar</id>
            <activation>
                <activeByDefault>true</activeByDefault>
            </activation>
        </profile>
     </profiles>
</settings>

```
4. run mvn clean verify sonar:sonar
5. We will get  Connection refused error
6. login into sonar http://localhost:9000/
7. Click on "A" i.e. Administrator -> Security -> Token -> copy the token
8. run mvn clean verify sonar:sonar -Dsonar.login=75910d3ec08e73bef04aa41863b934ba9ca7119e
9. This will now analyse and send the report to sonar



[Repository_Managers](Repository_Manager.md)

[PLugins](Plugins.md)














 
    
    
    
    
     
    


 










 