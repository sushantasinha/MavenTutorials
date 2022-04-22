# Maven Plugins

How to create a maven plugins:

Code is present here:
Completed+Plugin+Projects.zip

We have create 2 proj... one is for plugin and another is to use plugin

Packaging for the project to create plugin is "maven-plugin" instead of jar/war
Plugin is set of Mojos (java file as like POJO). Each mojo represents a goal. A plugin can have multiple goals.

```
@Mojo(name = "info-renderer", defaultPhase = LifecyclePhase.COMPILE)
public class ProjectInfoMojo extends AbstractMojo {

    //Get ref of the proj whose details i need ... say i want to print all the meven dependencies used for the proj
    @Parameter(defaultValue = "${project}", required = true, readonly = true)
	MavenProject project;

    //Input parameter
    //Say we want all maven dependencies which is having "test" scope 
    //it can pass -Dzzz=xxx or in pom.xml (means the pom which is using this plugin)
	@Parameter(property = "scope")
	String scope;

	@Override
	public void execute() throws MojoExecutionException, MojoFailureException {
        ...	//goal logic
    }

}
```

goal name would be "info-renderer" and it is associated with "COMPILE" phase


Also need in pom:

```
    <build>
		<pluginManagement>
			<plugins>
				<plugin>
					<groupId>org.apache.maven.plugins</groupId>
					<artifactId>maven-plugin-plugin</artifactId>
					<version>${maven-plugin-plugin.version}</version>
				</plugin>
			</plugins>
		</pluginManagement>
	</build>
```

To test it, we dont need to create a new proj immediately... we can run mvn commend to test the plugin functionality

To run:

```
mvn groupId:artifactId:version:goal

mvn com.abc:projectinfo-maven-plugin:info-renderer

//shorthand syntx is, but to use this need this to settings.xml as below. If we miss that will get error

mvn projectinfo:info-renderer

```

```
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 https://maven.apache.org/xsd/settings-1.0.0.xsd">
    <pluginGroups>
        <pluginGroup>com.abc</pluginGroup>
    </pluginGroups>
</settings>
```

if we dont provide this in settings.xml maven by default will search for MOJO in 2 packages only:
org.apache.maven.plugins
org.codehaus.mojo


Now, if we want to use this plugin from another proj:

This proj archytype would be jar/war etc not "maven-plugin" 


```
<build>
	<plugins>
		<plugin>
			<groupId>com.bharath</groupId>
			<artifactId>projectinfo-maven-plugin</artifactId>
			<version>0.0.1-SNAPSHOT</version>
			<executions>
				<execution>
					<goals>
						<goal>info-renderer</goal>
					</goals>
				</execution>
			</executions>
			<configuration>
				<scope>test</scope> <-- this is the way to pass param ... we used param in our code and for that we need "scope" -->
			</configuration>
		</plugin>
	</plugins>

</build>

<properties>
	<maven.compiler.source>16</maven.compiler.source>
	<maven.compiler.target>16</maven.compiler.target>
	<java.version>16</java.version>
</properties>

```

Here we dont need to run below
```
mvn groupId:artifactId:version:goal

mvn com.abc:projectinfo-maven-plugin:info-renderer

//shorthand syntx is, but to use this need this to settings.xml as below. If we miss that will get error

mvn projectinfo:info-renderer

```

RATHER we just need to run clean compile (as stage was associated in MOJO is compile)








