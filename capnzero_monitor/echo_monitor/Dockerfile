#setup
FROM osrf/ros:melodic-desktop-full  
RUN apt-get update
RUN apt-get -y upgrade
RUN apt-get install -y apt-utils
RUN apt install -y wget

#install catkin build tools
RUN /bin/bash -c 'echo "deb http://packages.ros.org/ros/ubuntu `lsb_release -sc` main" > /etc/apt/sources.list.d/ros-latest.list'
RUN wget http://packages.ros.org/ros.key -O - | sudo apt-key add -
RUN apt-get update
RUN apt-get -y upgrade
RUN apt-get install -y python-catkin-tools


#install capnproto
RUN apt install -y capnproto libcapnp-dev
RUN curl -O https://capnproto.org/capnproto-c++-0.7.0.tar.gz
RUN tar zxf capnproto-c++-0.7.0.tar.gz
WORKDIR capnproto-c++-0.7.0
RUN /bin/bash -c './configure'
RUN make -j2 check
RUN make install
WORKDIR ..

#install conan
RUN apt install -y python-pip
RUN pip install conan

#install latest cmake
ADD https://github.com/Kitware/CMake/releases/download/v3.16.2/cmake-3.16.2-Linux-x86_64.sh /cmake-3.16.2-Linux-x86_64.sh
RUN mkdir /opt/cmake
RUN sh /cmake-3.16.2-Linux-x86_64.sh --prefix=/opt/cmake --skip-license
RUN ln -s /opt/cmake/bin/cmake /usr/local/bin/cmake

#install zmq
RUN echo "deb http://download.opensuse.org/repositories/network:/messaging:/zeromq:/git-draft/xUbuntu_18.04/ ./" >> /etc/apt/sources.list
RUN wget https://download.opensuse.org/repositories/network:/messaging:/zeromq:/git-draft/xUbuntu_18.04/Release.key -O- | apt-key add
RUN apt install -y libzmq3-dev
RUN apt update
RUN apt -y upgrade

#create workspace, copy git repo
RUN mkdir -p /catkin_ws/src
WORKDIR catkin_ws/src 
RUN git clone https://github.com/dasys-lab/capnzero-monitoring.git
WORKDIR capnzero-monitoring 

#change submodule from ssh to http
RUN sed -i "s|git@github.com:|https://github.com/|g" .gitmodules

#use cluster multicast address
RUN sed -i "s|127.0.0.1:7890|224.0.0.2:5555|g" example/monitored-subscriber/subscriber_main.cpp

RUN /bin/bash -c 'git submodule update --init'
WORKDIR ..
WORKDIR ..
RUN /bin/bash -c '. /opt/ros/melodic/setup.bash; catkin build'

#copy monitored-subscriber into capnzero folder
RUN cp -b build/capnzero-monitoring/bin/monitored-subscriber devel/lib/capnzero

CMD /bin/bash -c 'source devel/setup.bash; rosrun capnzero monitored-subscriber'
