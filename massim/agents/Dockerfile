FROM maven:3-jdk-11 as mavenBuilder

RUN git clone https://github.com/ajahl/massim_2019.git
WORKDIR massim_2019
RUN mvn package
WORKDIR javaagents
RUN sed -i "s|localhost|server-service|g" conf/BasicAgents/eismassimconfig.json
WORKDIR ../..

FROM alpine

WORKDIR /root/
COPY --from=mavenBuilder /massim_2019/javaagents .
RUN apk add --no-cache openjdk11
CMD ["java","-jar","target/javaagents-2019-1.0-jar-with-dependencies.jar","conf/BasicAgents"]
