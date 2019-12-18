FROM osrf/ros:melodic-desktop-full
RUN apt-get update
Run apt-get -y upgrade
RUN apt install -y capnproto libcapnp-dev

RUN mkdir -p /catkin_ws/src
WORKDIR catkin_ws/src
RUN git clone https://github.com/dasys-lab/capnzero.git
WORKDIR capnzero
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
RUN /bin/bash -c '. /opt/ros/melodic/setup.bash; catkin_make'
CMD /bin/bash -c 'source devel/setup.bash; rosrun capnzero pub topic "hello world"'
