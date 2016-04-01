GrandParent Pom [![Build Status](https://travis-ci.org/bhdrkn/grand-parent-pom.png?branch=master)](https://travis-ci.org/bhdrkn/grand-parent-pom)
====
## What is grand-parent-pom?
<img align="right" src="assets/shall_not_pass.gif">
GrandParent Pom is a parent pom that combines code quality plugins (**jacoco**, **checkstyle** and **findbugs**). Plugins check test coverage, code structure and well-know bugs. If any of coding standard is not met, build is going to be failed. By doing this, developers are forced to follow codding standards and best practices. 

### What about Sonar?
Sonar does almost same checks. But sonar does not fail builds. Usually, sonar checks are triggered and inspected after code is pushed to remote repository throgh a CDI pipeline. At that point, code is already submitted to upstream.

## How to use it?
Add this project as a parent to your parent pom.

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  
  <parent>
      <groupId>com.bahadirakin</groupId>
      <artifactId>grand-parent-pom</artifactId>
      <version>1.1</version>
  </parent>
  ...
</project>
```
After adding this, on your next build all the checks are automaticaly run.

### Jacoco
[Jacoco](http://eclemma.org/jacoco/) maven plugin calculates code coverage. grand-parent-pom configures Jacoco maven plugin to force branch and line coverage for each class. Users can configure minimum code coverages. 
* **rule.branch.coverage:** Each class' branch coverage. Default is ```0.50```
* **rule.line.coverage:** Each class' line coverage. Default is ```0.50```

#### How to change coverage rates?
These properties can be configured on child classes. For example, to set line and branch coverage to ```0.60```:
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  
  <parent>
      <groupId>com.bahadirakin</groupId>
      <artifactId>grand-parent-pom</artifactId>
      <version>1.0</version>
  </parent>
  ...
  <properties>
    ...
    <rule.branch.coverage>0.60</rule.branch.coverage>
    <rule.line.coverage>0.60</rule.line.coverage>
    ...
  </properties>
  ...
</project>
```
#### Disable Test Coverage Checks

To disable test coverage chekcs, ```skip.jacoco.check``` property can be used. This property can be used both in command line and in your ```pom.xml```.

```bash
$ mvn clean install $skip.jacoco.check
```
### CheckStyle
[Checkstyle](http://checkstyle.sourceforge.net/) checks general code structure. All the rules are embedded to ***grand-parent-pom*** xml. Checkstyle rules are determined using:
* [CheckStyle for Google's conventions](http://checkstyle.sourceforge.net/google_style.html)
* [CheckStyle for Sun's conventions](http://checkstyle.sourceforge.net/sun_style.html)
* [CheckStyle of SpringBoot](https://github.com/spring-projects/spring-boot/blob/master/spring-boot-parent/src/checkstyle/checkstyle.xml)

#### Suppress some checks?
Users can suppress some of the checks during development. For example, to suppress ```VisibilityModifier``` check you can use comments like this.
```java
@Component
public class SchedulerController {

    //CHECKSTYLE.OFF: VisibilityModifier - Because bla bla bla
    @Autowired
    Scheduler scheduler;
    //CHECKSTYLE.ON: VisibilityModifier
    
    ...
}
```

#### Disable CheckStyle Checks
To disable CheckStyle chekcs, ```skip.checkstyle.check``` property can be used. This property can be used both in command line and in your ```pom.xml```.

```bash
$ mvn clean install $skip.checkstyle.check
```

### Findbugs
[Findbugs](http://findbugs.sourceforge.net/) checks common patterns that leads bugs. 

#### Suppress some rules?
To suppress rules, grand-parent-pom provides additional annotations for findbugs. Here is an example for suppressing [SE_NO_SERIALVERSIONID](http://findbugs.sourceforge.net/bugDescriptions.html#SE_NO_SERIALVERSIONID)

```java
@SuppressFBWarnings(value = "SE_NO_SERIALVERSIONID",justification = "because bla bla bla....")
public class MyClass implements Serializable {
  ...
}
```
#### Disable Findbugs Checks
To disable Findbugs chekcs, ```skip.findbugs.check``` property can be used. This property can be used both in command line and in your ```pom.xml```.

```bash
$ mvn clean install $skip.findbugs.check
```


