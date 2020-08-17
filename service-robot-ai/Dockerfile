FROM osrf/ros:melodic-desktop-full
RUN apt-get update
RUN apt-get -y upgrade
RUN apt install -y wget
RUN apt-get install -y libsfml-dev

#install catkin build tools
RUN /bin/bash -c 'echo "deb http://packages.ros.org/ros/ubuntu `lsb_release -sc` main" > /etc/apt/sources.list.d/ros-latest.list'
RUN wget http://packages.ros.org/ros.key -O - | apt-key add -
RUN apt-get update
RUN apt-get -y upgrade
RUN apt-get install -y python-catkin-tools

RUN apt install -y net-tools openjdk-11-jdk myrepos autoconf automake libtool cmake bison re2c build-essential g++ gitk uuid-dev capnproto libcapnp-dev python-catkin-tools qtscript5-dev

RUN echo "deb http://download.opensuse.org/repositories/network:/messaging:/zeromq:/git-draft/xUbuntu_18.04/ ./" >> /etc/apt/sources.list
RUN wget --no-check-certificate https://download.opensuse.org/repositories/network:/messaging:/zeromq:/git-draft/xUbuntu_18.04/Release.key -O- | apt-key add
RUN apt install -y libzmq3-dev

RUN apt install -y capnproto libcapnp-dev

RUN mkdir catkin_ws
WORKDIR catkin_ws

RUN mkdir src

WORKDIR src

RUN git clone https://github.com/potassco/clingo.git
WORKDIR clingo/
RUN git checkout -b kub aee5dacc3faed4e47f1efba05733e962fd15a4ce
RUN mkdir build
RUN git submodule update --init --recursive
WORKDIR build
RUN cmake ..
RUN make
RUN make install
WORKDIR ../..

RUN git clone https://github.com/dasys-lab/alica.git
RUN git clone https://github.com/dasys-lab/alica-plan-designer-fx.git
RUN git clone https://github.com/dasys-lab/alica-supplementary.git
RUN git clone https://github.com/dasys-lab/aspsuite.git
RUN git clone https://github.com/dasys-lab/capnzero.git
RUN git clone https://github.com/dasys-lab/essentials.git
RUN git clone https://github.com/dasys-lab/service-robot-ai.git
RUN git clone https://github.com/dasys-lab/service-robot-grid-simulation.git
RUN git clone https://github.com/dasys-lab/tmxparser.git

WORKDIR tmxparser
RUN mkdir build
WORKDIR build
RUN cmake .. -DBUILD_TINYXML2=ON
RUN make
RUN make install
WORKDIR ..
WORKDIR ..

WORKDIR alica
RUN git checkout newPD
RUN git checkout -b kub_newPD a07c1c00181059b6add4bc04d97f0396e281aa5d
WORKDIR ..

WORKDIR alica-supplementary
RUN git checkout CapnZero
RUN git checkout -b kub_CapnZero ecc9cc5d0cb4c86e93c131c47a8d314bd2e763e5
WORKDIR ..

WORKDIR aspsuite
RUN git checkout srg_dev
RUN git checkout -b kub_srg_dev 3f547293ee26faf80169a453cbc5d32507376a88
WORKDIR ..

WORKDIR capnzero
RUN git checkout -b kub_srai eab8d9e840d3e0017fdd8e03689c136213a609e6
WORKDIR ..

WORKDIR essentials
RUN git checkout CapnZero
RUN git checkout -b kub_CapnZero 61170f55780e1a2b66ab18d29661c703b5b96d52
WORKDIR ..

WORKDIR alica-plan-designer-fx
RUN git checkout master
RUN git checkout -b kub 312b7b205f2a15c2f9df0c7c2838f4dcbb88718d
WORKDIR ..

WORKDIR service-robot-ai
RUN git checkout capnzero
RUN git checkout -b kub_capnzero 84c42ec50f3fb0a52854c8c3d3c0154bfec048fc
RUN sed -i "s|this->gui = new srg::GUI(this->agentName);|//this->gui = new srg::GUI(this->agentName);|g" srg_worldmodel/src/SRGWorldModel.cpp
WORKDIR ..

WORKDIR service-robot-grid-simulation
RUN git checkout -b kub 22c2ead171fb4f0fcecdc5584e92b275553efbdd
WORKDIR ..

WORKDIR ..
RUN apt update
RUN apt -y upgrade
RUN /bin/bash -c '. /opt/ros/melodic/setup.bash; catkin build'
WORKDIR src/service-robot-ai
CMD /bin/bash -c 'source ../../devel/setup.bash; export uid=1000 gid=1000; rosrun srg_base srg_base -m "Human"'
