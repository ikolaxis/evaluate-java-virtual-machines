# evaluate-java-virtual-machines
Contains the Dockerfiles that I used to evaluate how HotSpot VM performs compared to OpenJ9 VM.

Every Dockerfile in this repository runs the well-known application [Spring-PetClinic](https://github.com/spring-projects/spring-petclinic) on top of a different Java Virtual Machine (JVM), to allow us to assess the performance of each JVM.

## Java 8 - HotSpot
To run with [Java 8 - HotSpot](https://github.com/ikolaxis/evaluate-java-virtual-machines/blob/master/java8/hotspot/Dockerfile):
```
docker run --name java8-hotspot --rm -it -p 8081:8080 ikolaxis/evaluate-java-virtual-machines:java8-hotspot
```

## Java 8 - OpenJ9
To run with [Java 8 - OpenJ9](https://github.com/ikolaxis/evaluate-java-virtual-machines/blob/master/java8/openj9/Dockerfile):
```
docker run --name java8-openj9 --rm -it -p 8082:8080 ikolaxis/evaluate-java-virtual-machines:java8-openj9
```

## Java 11 - HotSpot (default)
To run with [Java 11 - HotSpot](https://github.com/ikolaxis/evaluate-java-virtual-machines/blob/master/java11/hotspot/default/Dockerfile):
```
docker run --name java11-hotspot --rm -it -p 8083:8080 ikolaxis/evaluate-java-virtual-machines:java11-hotspot
```
## Java 11 - HotSpot (jlinked)
To run with the minimum required [Java 11 - HotSpot](https://github.com/ikolaxis/evaluate-java-virtual-machines/blob/master/java11/hotspot/jlinked/Dockerfile), which was assembled using [jlink](https://openjdk.java.net/jeps/282):
```
docker run --name java11-hotspot-jlinked --rm -it -p 8084:8080 ikolaxis/evaluate-java-virtual-machines:java11-hotspot-jlinked
```
## Java 11 - OpenJ9 (default)
To run with [Java 11 - OpenJ9](https://github.com/ikolaxis/evaluate-java-virtual-machines/blob/master/java11/openj9/default/Dockerfile):
```
docker run --name java11-openj9 --rm -it -p 8085:8080 ikolaxis/evaluate-java-virtual-machines:java11-openj9
```
## Java 11 - OpenJ9 (jlinked)
To run with the minimum required [Java 11 - OpenJ9](https://github.com/ikolaxis/evaluate-java-virtual-machines/blob/master/java11/openj9/jlinked/Dockerfile), which was assembled using [jlink](https://openjdk.java.net/jeps/282):
```
docker run --name java11-openj9-jlinked --rm -it -p 8086:8080 ikolaxis/evaluate-java-virtual-machines:java11-openj9-jlinked
```
## Java 11 - OpenJ9 (Class Data Sharing)
For this experiment, we assume that the shared classes will be cached under directory /tmp.
To run with [Java 11 - OpenJ9 - Class Data Sharing](https://github.com/ikolaxis/evaluate-java-virtual-machines/blob/master/java11/openj9/shareclasses/Dockerfile):
```
docker run --rm -it -p 8087:8080 -v /tmp:/app/shareclasses ikolaxis/evaluate-java-virtual-machines:java11-openj9-shareclasses
```
To notice a reduction in memory footprint and startup time, you will need to run more than one instances of the same application, each one running on a different port.
