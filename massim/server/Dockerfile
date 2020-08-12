FROM maven:3-jdk-11 as mavenBuilder

RUN git clone https://github.com/ajahl/massim_2019.git
WORKDIR massim_2019
RUN mvn package
WORKDIR ..

FROM alpine

WORKDIR /root/
COPY --from=mavenBuilder /massim_2019/server .
RUN apk add --no-cache openjdk11
CMD ["java","-jar","target/server-2019-2.1-jar-with-dependencies.jar","-conf","conf/SampleConfig.json","--monitor"]
