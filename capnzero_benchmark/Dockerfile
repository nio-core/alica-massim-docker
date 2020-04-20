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

RUN apt install -y capnproto libcapnp-dev

#uninstall default protobuf
RUN apt-get remove -y libprotobuf-dev

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

RUN mkdir -p /catkin_ws/src
WORKDIR catkin_ws/src
RUN git clone https://github.com/DiCoreSystems/ZeroMQEncoding.git
#RUN sed -i "s|224.0.0.2|capnzero|g" capnzero/capnzero/src/pub.cpp
WORKDIR ZeroMQEncoding
RUN rm -r capnzero_eval
RUN rm -r discovery
RUN rm -r discovery_msgs
RUN rm -r id_capnzero
RUN rm -r rosmq
RUN rm -r CapnZeroDebugUtils
WORKDIR ..
WORKDIR ..
RUN apt install -y wget
RUN rosdep install --from-path src
RUN echo "deb http://download.opensuse.org/repositories/network:/messaging:/zeromq:/git-draft/xUbuntu_18.04/ ./" >> /etc/apt/sources.list
RUN wget https://download.opensuse.org/repositories/network:/messaging:/zeromq:/git-draft/xUbuntu_18.04/Release.key -O- | apt-key add
RUN apt install -y libzmq3-dev
RUN apt update
RUN apt -y upgrade
RUN /bin/bash -c '. /opt/ros/melodic/setup.bash; catkin build'
CMD /bin/bash -c 'source devel/setup.bash; rosrun capnzero benchmark'
