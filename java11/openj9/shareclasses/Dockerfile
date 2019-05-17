# Stage 1: Download & Build Spring-PetClinic
FROM adoptopenjdk/openjdk11-openj9:latest as builder
LABEL maintainer "Ioannis L. Kolaxis"

RUN apt-get update
RUN apt-get install -y \
	git \
	maven

WORKDIR /tmp
RUN git clone https://github.com/spring-projects/spring-petclinic.git

WORKDIR /tmp/spring-petclinic
RUN mvn install

# Stage 2: Build the image that will be used for the Performance Tests
FROM adoptopenjdk/openjdk11-openj9:alpine-slim

WORKDIR /app
COPY --from=builder /tmp/spring-petclinic/target/spring-petclinic-2.1.0.BUILD-SNAPSHOT.jar /app

# The cached classes will be stored under this folder
WORKDIR /app/shareclasses

# Run the application with the Class Data Sharing (CDS) feature enabled
WORKDIR /app
CMD ["java","-Xshareclasses:cacheDir=/app/shareclasses","-jar","spring-petclinic-2.1.0.BUILD-SNAPSHOT.jar"]
