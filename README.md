# Gatling Test Packaged in a Jar

## Requirement

- Java 8
- SBT 0.13.8

## Running a simulation using SBT
- sbt "gatling-it:testOnly example.SomeGatlingSimulation"


## Packaging test as a jar and running the test
- sbt "it:assembly"
    - This will create a jar target/scala-2.11/gatling-tests-as-jar-1.0.jar
- To run the simulation
    - `java -jar target/scala-2.11/gatling-tests-as-jar-1.0.jar -s example.SomeGatlingSimulation`
    - The jar accepts the same [command line options](http://gatling.io/docs/2.2.2/general/configuration.html#command-line-options) that gatling accepts. 

- You can increase the number of users per second and the duration using environment variables. 
    - `export CONSTANT_USER_PER_SEC=10 DURATION="2 seconds"`
    
## Executing via RedLine13 Command Line
- Plugin
 - Name = sbtjava
 - Properties
  - sbtjava_simClass - simulation class embedded in jar
  
```
  curl -v https://www.redline13.com/Api/LoadTest \
    -H "X-Redline-Auth: XXXXXXXXX" \
    -F testType=gatling-test \
    -F name=GatlingFromAPI \
    -F "file=@gatling-tests-as-jar-1.0.jar" \
    -F opts="-DCONSTANT_USER_PER_SEC=10 -DDURATION='10 seconds'" \
    -F version=2.2.0 \
    -F numServers=1 \
    -F plugin[0]=sbtjava \
    -F sbtjava_simClass=example.SomeGatlingSimulation
```
