ARG PARENT_IMAGE="symbiflow-ql:latest"
FROM ${PARENT_IMAGE} as base

ARG DEBIAN_FRONTEND=noninteractive
ENV TZ=America/Los_Angeles

RUN apt-get update && apt-get install -y \
    libusb-1.0-0 \
    udev && \
    rm -rf /var/lib/apt/lists/*

FROM base as release-candidate

ARG USER=ic
ARG UID=1000
ARG GID=1000
RUN addgroup --gid ${GID} ${USER}
RUN adduser --uid ${UID} --gid ${GID} --gecos "" --disabled-password --shell /bin/bash ${USER} 
RUN usermod -a -G plugdev ${USER}
# COPY 99-fomu.rules /etc/udev/rules.d/99-fomu.rules
USER ${USER}
WORKDIR /home/${USER}

SHELL [ "/bin/bash" ]
COPY conda-make.sh /scripts/conda-make.sh
ENTRYPOINT [ "/scripts/conda-make.sh" ]

FROM release-candidate as symbiflow-ql-make

# Below are some example commands to build and run a docker container.
# From within the dockerfiles directory in a terminal window type:
#
# docker build . -t symbiflow-ql-make 
# 
# Or to base it on a particular parent image, try something like:
# 
# docker build --build-arg PARENT_IMAGE=symbiflow-ql:1.3.0 . -t symbiflow-ql-make:1.3.0
# 
# In a directory containing a Makefile that runs symbiflow tools:
# docker run --rm -v $(pwd):/home/ic symbiflow-ql-make
#
# The above will execute make within the running container.  Any
# tools called by the makefile will run within the container.
#
# You can run the container with an optional command, which it 
# passes to make.  For instance to run 'make clean' in the container:
#
# docker run --rm -v $(pwd):/home/ic symbiflow-ql-make clean
#
# Or if you tagged the image with a version, try something like:
#
# docker run --rm -v $(pwd):/home/ic symbiflow-ql-make:1.3.0 all
#