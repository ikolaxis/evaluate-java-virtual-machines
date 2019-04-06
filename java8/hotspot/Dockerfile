FROM adoptopenjdk/openjdk8:alpine as builder
LABEL maintainer "Ioannis L. Kolaxis"

RUN apk update \
 && apk add git \
            maven 

WORKDIR /tmp
RUN git clone https://github.com/spring-projects/spring-petclinic.git

WORKDIR /tmp/spring-petclinic
RUN mvn install
WORKDIR /tmp/spring-petclinic/target

FROM adoptopenjdk/openjdk8:alpine-slim
WORKDIR /app
COPY --from=builder /tmp/spring-petclinic/target/spring-petclinic-2.1.0.BUILD-SNAPSHOT.jar /app
CMD ["java","-jar","spring-petclinic-2.1.0.BUILD-SNAPSHOT.jar"]
