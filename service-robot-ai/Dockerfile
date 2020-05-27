FROM osrf/ros:melodic-desktop-full
#update apt-get to install wget
RUN apt-get update
RUN apt-get -y upgrade
RUN apt-get install wget
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
RUN wget https://download.opensuse.org/repositories/network:/messaging:/zeromq:/git-draft/xUbuntu_18.04/Release.key -O- | apt-key add
RUN apt install -y libzmq3-dev

RUN apt install -y capnproto libcapnp-dev

RUN mkdir catkin_ws
WORKDIR catkin_ws

RUN mkdir src
#RUN catkin init

WORKDIR src

RUN git clone https://github.com/potassco/clingo.git
WORKDIR clingo/
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
WORKDIR ..

WORKDIR alica-supplementary
RUN git checkout CapnZero
WORKDIR ..

WORKDIR aspsuite
RUN git checkout srg_dev
WORKDIR ..

WORKDIR capnzero
RUN git checkout master
WORKDIR ..

WORKDIR essentials
RUN git checkout CapnZero
WORKDIR ..

WORKDIR alica-plan-designer-fx
RUN git checkout master
WORKDIR ..

WORKDIR service-robot-ai
RUN git checkout capnzero
WORKDIR ..

#RUN echo "source /opt/ros/melodic/setup.bash" >> ~/.bashrc
#RUN echo "source ~/PKVR/workspace/devel/setup.bash" >> ~/.bashrc
#RUN echo "#fancy prompt that also shows the current branch " >> ~/.bashrc
#RUN echo "export PS1='\[\033[01;32m\]\u@\h\[\033[01;34m\] \w \[\033[01;31m\]$(__git_ps1 "[%s]")\[\033[01;34m\]\$\[\033[00m\] '" >>  ~/.bashrc
#RUN /bin/bash -c 'source ~/.bashrc'

WORKDIR ..
RUN apt update
RUN apt -y upgrade
RUN /bin/bash -c '. /opt/ros/melodic/setup.bash; catkin build'
WORKDIR src/service-robot-ai
CMD /bin/bash -c 'source ../../devel/setup.bash; export uid=1000 gid=1000; rosrun srg_base srg_base -m "Human"'
