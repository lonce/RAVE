# docker image build --build-arg USER_ID=$(id -u) --build-arg GROUP_ID=$(id -g) --file Dockerfile --tag foo:bar ../
FROM nvcr.io/nvidia/pytorch:22.11-py3
#FROM nvcr.io/nvidia/pytorch:20.12-py3

# settings from upf: https://guiesbibtic.upf.edu/recerca/hpc/create-docker-image
MAINTAINER Lonce Wyse "https://registry.sb.upf.edu"
LABEL authors="Lonce Wyse lonce.wyse@upf.edu"
LABEL description="environment for DL training of MTCRNN"

# create a project directory inside user home
ENV PROJECT_DIR /working
RUN mkdir $PROJECT_DIR
WORKDIR $PROJECT_DIR

#conda create -n rave python=3.9
#RUN apt-get update \
# && DEBIAN_FRONTEND="noninteractive" apt-get -y install tzdata \
# && apt-get install -y python3.9

COPY requirements.txt .
RUN pip install PyYAML --ignore-installed
RUN pip install -r requirements.txt
RUN pip install jupyterlab

WORKDIR /RAVE

ARG USER_ID
ARG GROUP_ID

RUN addgroup --gid $GROUP_ID user
RUN adduser --disabled-password --gecos '' --uid $USER_ID --gid $GROUP_ID user
USER user

# default command will launch JupyterLab server for development
CMD ["bash"]

# To run - go to your rave directory, run
# docker run  --ipc=host --gpus "device=0" -it -v $(pwd):/RAVE  --rm lonce:rave
# Then to get the ip address of the docker container
# hostname -I
# remote:
#        jupyter lab  --ip 0.0.0.0 --port=8889 --no-browser