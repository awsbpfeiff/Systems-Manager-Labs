---
weight: 2
title: Explore Application
---

#### Explore the Sample Application<a name="xray-gettingstarted-sample"></a>

The sample application is an HTTP web API in Java that is configured to use the X\-Ray SDK for Java\. When you deploy the application to Elastic Beanstalk, it creates the DynamoDB tables, compiles the API with Gradle, and configures the nginx proxy server to serve the web app statically at the root path\. At the same time, Elastic Beanstalk routes requests to paths starting with `/api` to the API\.

To instrument incoming HTTP requests, the application adds the `TracingFilter` provided by the SDK\.

**Example src/main/java/scorekeep/WebConfig\.java \- Servlet Filter**  

```
import javax.servlet.Filter;
import [com\.amazonaws\.xray\.javax\.servlet\.AWSXRayServletFilter](https://docs.aws.amazon.com/xray-sdk-for-java/latest/javadoc/com/amazonaws/xray/javax/servlet/AWSXRayServletFilter.html);
...

@Configuration
public class WebConfig {

  @Bean
  public Filter TracingFilter() {
    return new AWSXRayServletFilter("Scorekeep");
  }
...
```

This filter sends trace data about all incoming requests that the application serves, including request URL, method, response status, start time, and end time\.

![\[Primary service node in console\]](http://docs.aws.amazon.com/xray/latest/devguide/images/scorekeep-servicemap-rootnode-gettingstarted.png)

The application also makes downstream calls to DynamoDB using the AWS SDK for Java\. To instrument these calls, the application simply takes the AWS SDK\-related submodules as dependencies, and the X\-Ray SDK for Java automatically instruments all AWS SDK clients\.

The application uses a `Buildfile` file to build the source code on\-instance with `Gradle` and a `Procfile` file to run the executable JAR that Gradle generates\. `Buildfile` and `Procfile` support is a feature of the [Elastic Beanstalk Java SE platform](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/java-se-platform.html)\.

**Example Buildfile**  

```
build: gradle build
```

**Example Procfile**  

```
web: java -Dserver.port=5000 -jar build/libs/scorekeep-api-1.0.0.jar
```

The `build.gradle` file downloads the SDK submodules from Maven during compilation by declaring them as dependencies\.

**Example build\.gradle \-\- Dependencies**  

```
...
dependencies {
    compile("org.springframework.boot:spring-boot-starter-web")
    testCompile('org.springframework.boot:spring-boot-starter-test')
    compile('com.amazonaws:aws-java-sdk-dynamodb')
    compile("com.amazonaws:aws-xray-recorder-sdk-core")
    compile("com.amazonaws:aws-xray-recorder-sdk-aws-sdk")
    compile("com.amazonaws:aws-xray-recorder-sdk-aws-sdk-instrumentor")
    ...
}
dependencyManagement {
    imports {
        mavenBom("com.amazonaws:aws-java-sdk-bom:1.11.67")
        mavenBom("com.amazonaws:aws-xray-recorder-sdk-bom:2.2.0")
    }
}
```

The core, AWS SDK, and AWS SDK Instrumentor submodules are all that's required to automatically instrument any downstream calls made with the AWS SDK\.

To run the X\-Ray daemon, the application uses another feature of Elastic Beanstalk, configuration files\. The configuration file tells Elastic Beanstalk to run the daemon and send its log on demand\.

**Example \.ebextensions/xray\.config**  

```
option_settings:
  aws:elasticbeanstalk:xray:
    XRayEnabled: true

files:
  "/opt/elasticbeanstalk/tasks/taillogs.d/xray-daemon.conf" :
    mode: "000644"
    owner: root
    group: root
    content: |
      /var/log/xray/xray.log
```

The X\-Ray SDK for Java provides a class named `AWSXRay` that provides the global recorder, a `TracingHandler` that you can use to instrument your code\. You can configure the global recorder to customize the `AWSXRayServletFilter` that creates segments for incoming HTTP calls\. The sample includes a static block in the `WebConfig` class that configures the global recorder with plugins and sampling rules\.

**Example src/main/java/scorekeep/WebConfig\.java \- Recorder**  

```
import [com\.amazonaws\.xray\.AWSXRay](https://docs.aws.amazon.com/xray-sdk-for-java/latest/javadoc/com/amazonaws/xray/AWSXRay.html);
import [com\.amazonaws\.xray\.AWSXRayRecorderBuilder](https://docs.aws.amazon.com/xray-sdk-for-java/latest/javadoc/com/amazonaws/xray/AWSXRayRecorderBuilder.html);
import [com\.amazonaws\.xray\.plugins\.EC2Plugin](https://docs.aws.amazon.com/xray-sdk-for-java/latest/javadoc/com/amazonaws/xray/plugins/EC2Plugin.html);
import [com\.amazonaws\.xray\.plugins\.ElasticBeanstalkPlugin](https://docs.aws.amazon.com/xray-sdk-for-java/latest/javadoc/com/amazonaws/xray/plugins/ElasticBeanstalkPlugin.html);
import [com\.amazonaws\.xray\.strategy\.sampling\.LocalizedSamplingStrategy](https://docs.aws.amazon.com/xray-sdk-for-java/latest/javadoc/com/amazonaws/xray/strategy/sampling/LocalizedSamplingStrategy.html);

@Configuration
public class WebConfig {
...
  static {
    AWSXRayRecorderBuilder builder = AWSXRayRecorderBuilder.standard().withPlugin(new EC2Plugin()).withPlugin(new ElasticBeanstalkPlugin());

    URL ruleFile = WebConfig.class.getResource("/sampling-rules.json");
    builder.withSamplingStrategy(new LocalizedSamplingStrategy(ruleFile));

    AWSXRay.setGlobalRecorder(builder.build());
  }
}
```

This example uses the builder to load sampling rules from a file named `sampling-rules.json`\. Sampling rules determine the rate at which the SDK records segments for incoming requests\. 

**Example src/main/java/resources/sampling\-rules\.json**  

```
{
  "version": 1,
  "rules": [
    {
      "description": "Resource creation.",
      "service_name": "*",
      "http_method": "POST",
      "url_path": "/api/*",
      "fixed_target": 1,
      "rate": 1.0
    },
    {
      "description": "Session polling.",
      "service_name": "*",
      "http_method": "GET",
      "url_path": "/api/session/*",
      "fixed_target": 0,
      "rate": 0.05
    },
    {
      "description": "Game polling.",
      "service_name": "*",
      "http_method": "GET",
      "url_path": "/api/game/*/*",
      "fixed_target": 0,
      "rate": 0.05
    },
    {
      "description": "State polling.",
      "service_name": "*",
      "http_method": "GET",
      "url_path": "/api/state/*/*/*",
      "fixed_target": 0,
      "rate": 0.05
    }
  ],
  "default": {
    "fixed_target": 1,
    "rate": 0.1
  }
}
```

The sampling rules file defines four custom sampling rules and the default rule\. For each incoming request, the SDK evaluates the custom rules in the order in which they are defined\. The SDK applies the first rule that matches the request's method, path, and service name\. For Scorekeep, the first rule catches all POST requests \(resource creation calls\) by applying a fixed target of one request per second and a rate of 1\.0, or 100 percent of requests after the fixed target is satisfied\.

The other three custom rules apply a five percent rate with no fixed target to session, game, and state reads \(GET requests\)\. This minimizes the number of traces for periodic calls that the front end makes automatically every few seconds to ensure the content is up to date\. For all other requests, the file defines a default rate of one request per second and a rate of 10 percent\.

The sample application also shows how to use advanced features such as manual SDK client instrumentation, creating additional subsegments, and outgoing HTTP calls\. For more information, see [AWS X\-Ray Sample Application](xray-scorekeep.md)\.

#### Clean Up<a name="xray-gettingstarted-cleanup"></a>

Terminate your Elastic Beanstalk environment to shut down the Amazon EC2 instances, DynamoDB tables, and other resources\.

**To terminate your Elastic Beanstalk environment**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the [management console](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/environments-console.html) for your environment\.

1. Choose **Actions**\.

1. Choose **Terminate Environment**\.

1. Choose **Terminate**\.

Trace data is automatically deleted from X\-Ray after 30 days\.

**End of Lab Exercises** 
 
**Thank you for using this lab.** 
 