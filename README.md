# Karate Gatling
## API Perf-Testing Made `Simple.`

# Index

<table>
<tr>
  <th>Start</th>
  <td>
      <a href="#maven">Maven</a>
    | <a href="#gradle">Gradle</a>
    | <a href="#logging">Logging</a>
    | <a href="#limitations">Limitations</a>
    | <a href="#usage">Usage</a>    
  </td>
</tr>
<tr>
  <th>API</th>
  <td>
      <a href="#karateprotocol"><code>karateProtocol()</code></a>
    | <a href="#nameresolver"><code>nameResolver</code></a>
    | <a href="#pausefor"><code>pauseFor()</code></a>
    | <a href="#runner"><code>runner</code></a>
    | <a href="#karatefeature"><code>karateFeature()</code></a>
    | <a href="#karateset"><code>karateSet()</code></a>
    | <a href="#tag-selector">Tag Selector</a>
    | <a href="#ignore-tags">Ignore Tags</a>
  </td>
</tr>
<tr>
  <th>Advanced</th>
  <td>
      <a href="#gatling-session">Session</a>
    | <a href="#feeders">Feeders</a>
    | <a href="#chaining">Chaining</a>
    | <a href="#karatecallsingle"><code>karate.callSingle()</code></a>
    | <a href="#detecting-gatling-at-run-time">Detecting Gatling At Run Time</a>
    | <a href="#think-time">Think Time</a>
    | <a href="#configure-localaddress"><code>configure localAddress</code></a>
    | <a href="#custom">Profiling Custom Java Code</a>
    | <a href="#captureperfevent"><code>PerfContext.capturePerfEvent()</code></a>
    | <a href="#increasing-thread-pool-size">Increasing Thread Pool Size</a>
    | <a href="#distributed-testing">Distributed Testing</a>   
  </td>
</tr>
</table>

### Capabilities
* Re-use Karate tests as performance tests executed by [Gatling](https://gatling.io)
* Use Gatling (and Scala) only for defining the load-model, everything else can be in Karate
* Karate assertion failures appear in Gatling report, along with the line-numbers that failed
* Leverage Karate's powerful assertion capabilities to check that server responses are as expected under load - which is much harder to do in Gatling and other performance testing tools
* API invocation sequences that represent end-user workflows are much easier to express in Karate
* [*Anything*](#custom) that can be written in Java can be performance tested !
* Option to scale out by [distributing a test](#distributed-testing) over multiple hardware nodes or Docker containers

## Demo Video
Refer: https://twitter.com/ptrthomas/status/986463717465391104


### Maven

> also see [using a Maven Profile](#using-a-maven-profile).

```xml
<dependency>
    <groupId>com.intuit.karate</groupId>
    <artifactId>karate-gatling</artifactId>
    <version>${karate.version}</version>
    <scope>test</scope>
</dependency>  
```

You will also need the [Gatling Maven Plugin](https://github.com/gatling/gatling-maven-plugin) and the [Scala Maven Pugin](https://github.com/davidB/scala-maven-plugin). Refer to the [sample project](../examples/gatling) for how to use this for a typical Karate project where feature files are in `src/test/java`. For convenience we recommend you keep even the Gatling simulation files in the same folder hierarchy, even though they are technically files with a `*.scala` extension.

```xml
  <plugin>
      <groupId>io.gatling</groupId>
      <artifactId>gatling-maven-plugin</artifactId>
      <version>${gatling.plugin.version}</version>
      <configuration>
          <simulationsFolder>src/test/java</simulationsFolder>
          <includes>
              <include>mock.CatsKarateSimulation</include>
          </includes>
      </configuration>               
  </plugin>
```

To run the Gatling test:

```
mvn clean test-compile gatling:test
```

And in case you have multiple Gatling simulation files and you want to choose only one to run:

```
mvn clean test-compile gatling:test -Dgatling.simulationClass=mock.CatsKarateSimulation
```

It is worth calling out that in the sample project, we are perf-testing [Karate test-doubles](https://hackernoon.com/api-consumer-contract-tests-and-test-doubles-with-karate-72c30ea25c18) ! A truly self-contained demo.
