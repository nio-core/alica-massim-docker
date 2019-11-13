FROM ubuntu:18.04

#install
RUN apt-get update
RUN apt-get upgrade -y
RUN apt-get install -y git
RUN apt-get install -y maven

RUN git clone https://github.com/ajahl/massim_2019.git
WORKDIR massim_2019
RUN mvn package
WORKDIR javaagents
CMD ["java","-jar","target/javaagents-2019-1.0-jar-with-dependencies.jar","0"]
