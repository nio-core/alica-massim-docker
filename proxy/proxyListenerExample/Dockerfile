FROM osrf/ros:melodic-desktop-full

#update apt-get to install wget
RUN apt-get update
RUN apt-get -y upgrade
RUN apt-get install wget

#install catkin build tools
RUN /bin/bash -c 'echo "deb http://packages.ros.org/ros/ubuntu `lsb_release -sc` main" > /etc/apt/sources.list.d/ros-latest.list'
RUN wget http://packages.ros.org/ros.key -O - | apt-key add -
RUN apt-get update
RUN apt-get -y upgrade
RUN apt-get install -y python-catkin-tools
RUN apt install -y autoconf
RUN apt-get install -y libtool libtool-bin

RUN apt install -y capnproto libcapnp-dev

#uninstall default protobuf
RUN apt-get remove -y libprotobuf-dev

RUN apt install -y net-tools openjdk-11-jdk myrepos autoconf automake libtool cmake bison re2c build-essential g++ gitk uuid-dev capnproto libcapnp-dev python-catkin-tools qtscript5-dev


# Install protoc
ENV PROTOBUF_URL https://github.com/google/protobuf/releases/download/v3.11.2/protobuf-cpp-3.11.2.tar.gz
RUN curl -L -o /tmp/protobuf.tar.gz $PROTOBUF_URL
WORKDIR /tmp/
RUN tar xvzf protobuf.tar.gz
WORKDIR /tmp/protobuf-3.11.2
RUN ./autogen.sh && \
    ./configure --prefix=/usr && \
    make clean && \
    make -j 3 && \
    make check && \
    make install
RUN ldconfig
WORKDIR ..
WORKDIR ..

#install java
RUN apt-get install -y openjdk-11-jre 

RUN git clone https://github.com/msgpack/msgpack-c.git
WORKDIR msgpack-c
RUN git checkout cpp_master
RUN cmake -DMSGPACK_CXX[11]=ON .
RUN make install
WORKDIR ..

RUN mkdir -p /catkin_ws/src
WORKDIR catkin_ws/src
RUN git clone https://github.com/DiCoreSystems/ZeroMQEncoding.git
RUN git clone https://github.com/dasys-lab/essentials

WORKDIR essentials
RUN git checkout -b kub_CapnZero 61170f55780e1a2b66ab18d29661c703b5b96d52
WORKDIR ..

#use this instead of git clone for testing local source code
#COPY /zeromq_encodings_ws /zeromq_encodings_ws
#WORKDIR /zeromq_encodings_ws
#RUN rm -r devel build
#WORKDIR src/

WORKDIR ZeroMQEncoding
RUN git checkout capnzero-monitoring-minikube
RUN sed -i "s|127.0.0.1:12345|192.168.39.103:31234|g" ZeroMQEncoding/capnzero/src/proxyListenerExample.cpp
RUN rm -r capnzero_eval
RUN rm -r discovery
RUN rm -r discovery_msgs
RUN rm -r rosmq
RUN rm -r CapnZeroDebugUtils
#RUN rm -r id_capnzero
WORKDIR ..
WORKDIR ..
RUN apt install -y wget
#RUN rosdep install --from-path src
RUN echo "deb http://download.opensuse.org/repositories/network:/messaging:/zeromq:/git-draft/xUbuntu_18.04/ ./" >> /etc/apt/sources.list
RUN wget https://download.opensuse.org/repositories/network:/messaging:/zeromq:/git-draft/xUbuntu_18.04/Release.key -O- | apt-key add
RUN apt install -y libzmq3-dev
RUN apt-get install -y libsfml-dev
RUN apt update
RUN apt -y upgrade
RUN /bin/bash -c '. /opt/ros/melodic/setup.bash; catkin build'
CMD /bin/bash -c 'source devel/setup.bash; rosrun capnzero proxyListenerExample'
