# podman build -t vnyazure .
# podman run -ti --name azure vnyazure
# podman start -ai azure

FROM debian:bullseye
ARG DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y \
    python3 sudo curl git \
    gnupg

RUN curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add - && \
    echo "deb [arch=amd64] https://packages.microsoft.com/repos/microsoft-debian-bullseye-prod bullseye main" > /etc/apt/sources.list.d/microsoft.list  && \
    apt-get update && apt-get install -y azure-cli powershell

RUN useradd -m -s /bin/bash viny 

USER viny
WORKDIR /home/viny
