# SonarQube Stash (BitBucket) plugin

**SonarQube is now a real reviewer!**
SonarQube Stash (BitBucket) plugin is a pull-request decorator which allows to integrate SonarQube violations directly into your pull-request.

![Screenshot SonarQube plugin](resources/Stash-plugin-issues.PNG)

## Getting started

#### Prerequisites
- Git client to checkout the code
- Maven 3.0.5
- JDK 1.8
- SonarQube 5.6.1 (LTS)
- Stash (BitBucket) 4.x

#### To build the plugin
This command generates a jar file.
```
mvn clean install
```

#### To deploy the plugin
Just copy the sonar-stash-plugin jar file to the plugin folder of the expected SonarQube server and restart the SonarQube server. For instance, on Linux platform:
```
~> cp target/sonar-stash-plugin-*.jar $SONARQUBE_HOME/extensions/plugins
```

#### Configuration on SonarQube server

* **sonar.stash.url** - HTTP URL of Stash instance
* **sonar.stash.login** - To define user to push violations on Stash pull-request. User must have **REPO_READ permission** for the repository. 
* **sonar.stash.password** - User password to push data on Stash instance.
* **sonar.stash.timeout** - To timeout when Stash Rest api does not replied with expected. Default value is 100000 ms.
* **sonar.stash.reviewer.approval** - SonarQube is able to approve the pull-request if there is no new issue introduced by the change. By default, this feature is deactivated: if activated, **Stash base user must have REPO_WRITE permission for the repositories.** 
* **sonar.stash.comments.reset** - If needed, you can reset comments published during the previous SonarQube analysis of your pull-request. Please notice only comments linked to the **sonar.stash.login** user will be deleted. This reset will be the first action performed by the plugin. By default this feature is activated.
* **sonar.stash.issue.threshold** - To limit the number of issue pushed to Stash.
* **sonar.stash.task.issue.severity.threshold** - SonarQube is able to create tasks for all issues with a severity higher to the threshold. By default, this feature is activated (threshold: MAJOR).
* **sonar.stash.include.analysis.overview** - Set to false to prevent creation of the analysis overview comment. By default, this feature is deactivated.
* **sonar.stash.include.existing.issues** - Set to true to include already existing issues on modified lines.
* **sonar.stash.project.base.dir** - base dir if source code is in different directory, for example "modules/". 

## How to run the plugin?

To activate the plugin, just add the following options to SonarQube launcher (for instance with sonar-runner):
```
sonar-runner -Dsonar.analysis.mode=issues -Dsonar.stash.notification -Dsonar.stash.project=<PROJECT> -Dsonar.stash.repository=<REPO> -Dsonar.stash.pullrequest.id=<PR_ID> ...
```