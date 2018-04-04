FROM ubuntu:latest

#LABEL version="1.6.5"
#LABEL maintainer "xma@spaceapplications.com"
LABEL description="This container contains CDFF"

RUN apt-get update && apt-get install -y build-essential \
apt-utils \
git \
cppcheck \
mono-devel \
wget \
unzip \
ruby \
wget

ENV PATH="/CDFF/External/install/bin:${PATH}"

# master
#ADD installers/infuse_environnement.env CDFF/External/installers/
#ADD fetch_compile_install_dependencies.sh CDFF/External

RUN wget http://www.rock-robotics.org/autoproj_bootstrap


# dependencies
RUN ruby autoproj_bootstrap git git@git.hb.dfki.de:infuse/buildconf branch=cdff_only
RUN .env.sh
RUN aup
RUN amake



