# ros2 openvino toolkit env master f1b1ca4d914186a1881b87f103be9c6e910c9d80

from osrf/ros:humble-desktop

# setting proxy env --option 
# If needed, enable the below ENV setting by correct proxies.
# ENV http_proxy=your_proxy
# ENV https_proxy=your_proxy

# maintainer information
LABEL maintainer="Pengqiang Li <pengqiang.li@intel.com>"

SHELL ["/bin/bash", "-c"]

# install openvino 2021.4
# https://docs.openvinotoolkit.org/latest/openvino_docs_install_guides_installing_openvino_apt.html
RUN apt update && apt install wget curl gnupg2 lsb-release
RUN wget -qO- https://apt.repos.intel.com/intel-gpg-keys/GPG-PUB-KEY-INTEL-SW-PRODUCTS.PUB | sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/intel.gpg
RUN echo "deb https://apt.repos.intel.com/openvino/2025 ubuntu20 main" | sudo tee /etc/apt/sources.list.d/intel-openvino-2025.list
RUN apt update

RUN apt-cache search openvino
RUN apt-get install -y openvino
#RUN ls -lh /opt/intel/openvino_2025
#RUN source /opt/intel/openvino_2021/bin/setupvars.sh 

# install librealsense2
RUN apt-get install -y --no-install-recommends \
software-properties-common 
# https://github.com/IntelRealSense/librealsense/blob/master/doc/distribution_linux.md
# Make sure you set http-proxy in below commands if your environment needs.
# RUN apt-key adv --keyserver-options http-proxy=your_proxy --keyserver keys.gnupg.net --recv-key F6E65AC044F831AC80A06380C8B3A55A6F3EFCDE || apt-key adv --keyserver-options http-proxy=your_proxy --keyserver hkp://keyserver.ubuntu.com:80 --recv-key F6E65AC044F831AC80A06380C8B3A55A6F3EFCDE 
#RUN apt-key adv --keyserver-options --keyserver keys.gnupg.net --recv-key F6E65AC044F831AC80A06380C8B3A55A6F3EFCDE || apt-key adv --keyserver-options --keyserver hkp://keyserver.ubuntu.com:80 --recv-key F6E65AC044F831AC80A06380C8B3A55A6F3EFCDE 
#RUN add-apt-repository "deb https://librealsense.intel.com/Debian/apt-repo focal main" -u \
#&& apt-get install -y --no-install-recommends \

RUN curl -sSf https://librealsense.intel.com/Debian/librealsense.pgp | sudo tee /etc/apt/keyrings/librealsense.pgp > /dev/null && \
echo "deb [signed-by=/etc/apt/keyrings/librealsense.pgp] https://librealsense.intel.com/Debian/apt-repo `lsb_release -cs` main" | \
sudo tee /etc/apt/sources.list.d/librealsense.list &&  apt-get update &&  apt-get install -y --no-install-recommends \
librealsense2-utils \
librealsense2-dev \
librealsense2-dbg \
libgflags-dev \
&& rm -rf /var/lib/apt/lists/*

RUN apt-get update && apt-get install -y sudo

# Set root password to "ubuntu"
RUN echo "root:ubuntu" | chpasswd

# Create a new user "ubuntu" with password "ubuntu"
RUN useradd -m -s /bin/bash ubuntu && \
    echo "ubuntu:ubuntu" | chpasswd && \
    usermod -aG sudo ubuntu

# Set working directory
WORKDIR /home/ubuntu

# Switch to user "ubuntu"
USER ubuntu

# build ros2 openvino toolkit
WORKDIR /home/ubuntu
RUN mkdir -p ros2_ws/src
WORKDIR /home/ubuntu/ros2_ws/src
RUN git clone https://github.com/intel/ros2_object_msgs.git
RUN git clone -b ros2 https://github.com/intel/ros2_openvino_toolkit.git
RUN git clone -b humble https://github.com/ros-perception/vision_opencv.git
WORKDIR /home/ubuntu/ros2_ws
#RUN source /opt/ros/humble/setup.bash && source /opt/intel/openvino_2021/bin/setupvars.sh && colcon build --cmake-args -DCMAKE_BUILD_TYPE=Release
