FROM adoptopenjdk/openjdk11-openj9:x86_64-ubuntu-jdk-11.0.2.9_openj9-0.12.1
LABEL maintainer "Ioannis L. Kolaxis"

RUN apt-get update
RUN apt-get install -y \
	git \
	maven

WORKDIR /tmp
RUN git clone https://github.com/spring-projects/spring-petclinic.git

WORKDIR /tmp/spring-petclinic
RUN mvn install

WORKDIR /tmp/spring-petclinic/target
RUN jlink --no-header-files --no-man-pages --compress=2 --strip-debug --add-modules java.base,java.logging,java.sql,java.naming,java.desktop,java.management,java.security.jgss,java.instrument,jdk.unsupported --output java-light

CMD ["java-light/bin/java","-jar","spring-petclinic-2.1.0.BUILD-SNAPSHOT.jar"]
