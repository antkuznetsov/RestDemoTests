This is an example of code coverage report executed against application running on remote server.
The project contains only tests. Application project is independant and separated from this project.

The application project sources are located [here](https://github.com/piczmar/RestDemo/tree/fa90dd4f98efa790ca9d0dd2c936f5d6938a0a2e)

### Running the project

```
mvn clean verify -Pstaging
```
Report is generated in `ft-staging/target/coverage-report`

### What happens under the hood:

1. `maven-dependency-plugin` is used to download sources and classes of the tested projects in build directory
2. `maven-surefire-plugin` is running tests against remote REST endpoints (application deployed on Tomcat configured with Jacoco agent)
3. `jacoco-maven-plugin` dumps the coverate data from remote server and saves in jacoco.exec file in build folder of this project
4. `maven-antrun-plugin` is used to generate report using Jacoco Ant target. This is needed because Macen Jacoco Plugin does not allow specification of source and class folders.

### Preparing tested project 
The tested project which is an independent project must expose source code.
This is done using maven by adding plugin to each module of the tested project (can be set on parent pom):
```
<plugin>
    <groupid>org.apache.maven.plugins</groupid>
    <artifactid>maven-source-plugin</artifactid>
    <executions>
        <execution>
            <id>attach-sources</id>
            <goals>
                <goal>jar</goal>
            </goals>
        </execution>
    </executions>
</plugin>
```

This way when project is built it will create not only jars with binaries but also with source code, e.g.:

```
com.consulner.springboot.rest.api-0.0.1-SNAPSHOT.jar
com.consulner.springboot.rest.api-0.0.1-SNAPSHOT-sources.jar
```

##References:
http://olafsblog.sysbsb.de/measuring-test-coverage-of-integration-tests-for-separated-modules-with-jacoco/
