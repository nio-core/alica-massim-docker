FROM ubuntu:18.04

#install
RUN apt-get update
RUN apt-get upgrade -y
RUN apt-get install -y git
RUN apt-get install -y maven

RUN git clone https://github.com/ajahl/massim_2019.git
WORKDIR massim_2019
RUN mvn package
WORKDIR server
CMD ["java","-jar","target/server-2019-2.1-jar-with-dependencies.jar","-conf","conf/SampleConfig.json"]
