# Copyright 2023 Roboware-org
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

FROM ubuntu:jammy-20221130

# Install prerequisites
RUN apt-get update  \
&& apt upgrade -y  \
&& apt-get install -y sudo software-properties-common curl gnupg2 lsb-release build-essential net-tools vim git python3-pip wget libudev-dev \
&& rm -rf /var/lib/apt/lists/* \
&& apt-get clean

ENV LANG=en_US.UTF-8
ENV TZ=Asia/Tokyo
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# Rust install
ENV RUST_HOME /usr/local/lib/rust
ENV RUSTUP_HOME ${RUST_HOME}/rustup
ENV CARGO_HOME ${RUST_HOME}/cargo
RUN mkdir /usr/local/lib/rust && \
    chmod 755 $RUST_HOME
RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs > ${RUST_HOME}/rustup.sh \
    && chmod +x ${RUST_HOME}/rustup.sh \
    && ${RUST_HOME}/rustup.sh -y --default-toolchain nightly --no-modify-path
ENV PATH $PATH:$CARGO_HOME/bin


# Install ROS 2 Humble
ENV ROS_PYTHON_VERSION=3
RUN apt-get install -y software-properties-common \
&& curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg \
&& echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" |  tee /etc/apt/sources.list.d/ros2.list > /dev/null \
&& apt update
RUN apt install -y ros-humble-ros-base \
&& apt install -y ros-humble-rmw-cyclonedds-cpp \
&& apt install -y ros-dev-tools \
&& rosdep init \
&& rosdep update

# Install safe_drive
RUN pip install git+https://github.com/tier4/colcon-cargo.git && pip install git+https://github.com/colcon/colcon-ros-cargo.git
RUN git clone https://github.com/tier4/cargo-ament-build.git && cd cargo-ament-build && cargo install --path .