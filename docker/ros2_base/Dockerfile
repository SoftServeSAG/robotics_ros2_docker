#FROM nvidia/opengl:1.0-glvnd-devel-ubuntu20.04
FROM nvidia/cuda:11.0-devel-ubuntu20.04

WORKDIR /root

# set tzdata and timezone to avoid build stuck
#ENV TZ=Europe/Kiev
#RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN locale-gen en_US en_US.UTF-8
RUN update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
RUN export LANG=en_US.UTF-8

# Distribution of ROS2
ARG distro="foxy"

# Specify the distribution of ROS2
ENV ROS_DISTRO $distro

RUN apt-get update -y && apt-get install -y \
    vim \
    tmux \
    tmuxinator \
    git \
    wget \
    lsb-release \
    lsb-core \
    vim \
    build-essential \
    ranger \
    g++ \
    net-tools \
    curl \
    gnupg 


RUN apt update -y && apt install -y curl gnupg2 lsb-release
RUN curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | apt-key add -
RUN sh -c 'echo "deb [arch=$(dpkg --print-architecture)] http://packages.ros.org/ros2/ubuntu $(lsb_release -cs) main" > /etc/apt/sources.list.d/ros2-latest.list'

# Add key
#RUN curl http://repo.ros2.org/repos.key | apt-key add -
#RUN sh -c 'echo "deb http://packages.ros.org/ros2/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros2-latest.list'
RUN sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
RUN apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654

RUN apt-get update && apt-get upgrade && apt-get install -y \
    ros-foxy-desktop \
    python3-catkin-pkg \
    python3-rospkg \
    python3-colcon-common-extensions \
    python3-rosdep \
    python3-argcomplete \
    python3-vcstool \ 
    && rm -rf /var/lib/apt/lists/*rm 

RUN rosdep init && rosdep fix-permissions && rosdep update

RUN sh -c 'pwd'
COPY docker/ros2_base/dotbashrc /root/dotbashrc
COPY docker/ros2_base/dottmux.conf /root/.tmux.conf

RUN echo 'export RUN_TMUX=true' >> ~/.bashrc
RUN echo 'source /root/dotbashrc' >> ~/.bashrc
RUN echo 'alias ra='"'"'ranger --choosedir=$HOME/.rangerdir; LASTDIR=`cat $HOME/.rangerdir`; cd "$LASTDIR"'"'"'' >> ~/.bashrc
