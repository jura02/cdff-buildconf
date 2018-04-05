FROM ubuntu:latest

#LABEL version="1.6.5"
#LABEL maintainer "xma@spaceapplications.com"
LABEL description="This container contains CDFF"

#apt install wget
#apt install ruby

RUN apt-get update && apt-get install -y build-essential \
ruby \
ruby-dev \
sudo \
wget
#apt-utils \
#git \
#cppcheck \
#mono-devel \
#unzip \


#ENV PATH="/CDFF/External/install/bin:${PATH}"

# master
#ADD installers/infuse_environnement.env CDFF/External/installers/
#ADD fetch_compile_install_dependencies.sh CDFF/External

RUN wget http://www.rock-robotics.org/autoproj_bootstrap


# dependencies
RUN ruby autoproj_bootstrap git git@gitlab.spaceapplications.com:InFuse/cdff-buildconf

RUN git config --global user.email "you@example.com"
RUN git config --global user.name "Your Name"


RUN source env.sh
RUN autoproj update
RUN autoproj build



