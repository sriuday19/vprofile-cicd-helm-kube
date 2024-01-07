FROM openjdk:8 AS BUILD_IMAGE
RUN apt-get update && apt-get install maven -y
RUN git clone https://github.com/sriuday19/vprofile-ci-docker.git
RUN cd vprofile-ci-docker && mvn install

FROM tomcat:8-jre11
LABEL Author="uday"
LABEL project="vprofile"

RUN rm -rf /usr/local/tomcat/webapps/*

COPY --from=BUILD_IMAGE vprofile-ci-docker/target/vprofile-v2.war /usr/local/tomcat/webapps/ROOT.war

EXPOSE 8080

CMD [ "catalina.sh", "run" ]