Java Notes
==========



Workflow
========

Running the app using maven and Tomcat
--------------------------------------

* References:
    * [mkyong: Create a maven java project].
	* [Maven: maven in 5 mins].
* Install JDK and maven:

```bash
sudo pacman -Syu install jdk10-openjdk maven
```

* Create a new project:

```bash
mvn archetype:generate -DgroupId={project-packaging} -DartifactId={project-name} -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
# eg.
mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=my-app -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
```

* Double check root `pom.xml` has the following lines (which may be missing).:

```xml
  <properties>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
  </properties>
```

* Build the application:

```bash
mvn package
```

* Test the built artefact (`.jar`):

```bash
java -cp target/my-app-1.0-SNAPSHOT.jar com.mycompany.app.App
```

### WAR artefacts on Tomcat

For web apps, they are commonly built as a WAR file, and run against an
external server (such as tomcat):

* Install tomcat:

```bash
sudo pacman -Syu tomcat8
```

* Deploy War artefact:

```bash
cp target/my-app-1.0-SNAPSHOT.war $TOMCAT_DIR/webapps
```

* Run the app:
    * The war package will be deployed as part of tomcat's webapps when tomcat is started. To start tomcat:
        ```bash
        cd $TOMCAT_DIR/bin
        ./startup.sh
        ```
    * The API URL for the rigel service will be: `http://localhost/my-app-1.0-SNAPSHOT/<API_path>`.
    * To deploy the service as the root node, copy the server `.war` to: `$TOMCAT_DIR/webapps/ROOT.war`. This will give a URL of: `http://localhost/<API_path>`.

Maven: Skipping test
--------------------

```bash
mvn package -Dmaven.test.skip=true
# or, for `surefire` package.
$ mvn package -DskipTests
```

Intellij
========

Intellij configuration
----------------------

* Install [CheckStyle plugin](https://github.com/jshiell/checkstyle-idea):
  File > Settings > Plugins > Browse repositories... > Search for
  `CheckStyle-IDEA` > Click `Install` button > Click `Restart Intellij IDEA` >
  Click `OK` > Click `Shutdown`.

Running the app from IntelliJ
-----------------------------

To run the my-app war artifact we need to setup a tomcat server in IntelliJ
that can deploy the artifact on start-up.

* Build the project by clicking `Build->Build Project`.
* Build the app artifacts by clicking `Build->Build Artifacts...`. In the
  pop-up, click `All Artifacts->Build`.
* In your IntelliJ menu click `Run->Edit Configurations...`.
* Click the `+` icon in the top left corner to add a configuration element.
* From the list, scroll down to `Tomcat Server->Local`.
* Enter a name for the tomcat server in the `Name` field e.g. `My-App`.
* In the `Tomcat Server Settings` section, enter the `HTTP port` number for
  tomcat.
* In the `Open browser` section, uncheck `After launch`.
* In the `VM options` field, you can enter custom memory
  values. eg. `-Xmx2048m -Xms1024m`.
* Click on the `Deployment` tab and click on the `+` icon on the right.
* Select `Artifact` as the deployment type.
* In the pop-up window, click on `my-app:war exploded` and click `OK`.
* Click `Apply` to save the changes.
* Close the `Edit Configurations...` window.
* Click `Run->My-App` to start the my-app server in Tomcat.
* Once `My-App` is ready you should see something like the following in your
  IntelliJ output window:

```
[2017-10-19 10:54:29,608] Artifact my-app:war exploded: Artifact is deployed successfully
[2017-10-19 10:54:29,609] Artifact my-app:war exploded: Deploy took 5,067 milliseconds
```

Running integration tests in IntelliJ
-------------------------------------

- To run integration tests for the my-app project, start the app in IntelliJ as
  described above.
- Right click on the integration test of interest and click **Run**.





[mkyong: Create a maven java project]: http://www.mkyong.com/maven/how-to-create-a-java-project-with-maven/
[Maven: maven in 5 mins]: https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html
