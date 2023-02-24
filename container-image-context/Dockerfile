FROM ubuntu:22.04
MAINTAINER Davide Franchi <https://github.com/Deido182>

# By default the user is root
# -y option just stands for "always yes" (the same of using "yes" command in pipe)
RUN apt-get -y upgrade
RUN apt-get -y update
RUN apt-get -y install podman
RUN apt-get -y install curl
RUN apt-get -y install vim
RUN apt-get -y install sudo

RUN echo '[registries.search]' >> /etc/containers/registries.conf
RUN echo 'registries = ["registry.access.redhat.com", "registry.redhat.io", "quay.io"]' >> /etc/containers/registries.conf
RUN echo '[registries.insecure]' >> /etc/containers/registries.conf
RUN echo 'registries = ["registry.access.redhat.com", "registry.redhat.io", "quay.io"]' >> /etc/containers/registries.conf
# The first are those considered by default when running 'search'
# The 'insecure' ones instead should not be used like this. 
# Anyway, without doing so you would be required to always run commands with '--tls-verify=false'

ENV PERS_STORAGE_DIR=/persistent-storage
ENV USER_PERS_STORAGE_DIR="${PERS_STORAGE_DIR}/user"
ENV USER_PERS_STORAGE_GROUP=user-pers-storage-group

RUN useradd -m user && echo "user:user" | chpasswd
RUN addgroup "${USER_PERS_STORAGE_GROUP}"
RUN adduser user "${USER_PERS_STORAGE_GROUP}"
# This is to make user a 'sudoer', if strictly needed
#RUN adduser user sudo

# Change default user shell
RUN usermod --shell /bin/bash user

COPY container-setup.sh /.setup.sh
ENTRYPOINT ["/bin/bash", "-c"]
CMD ["/.setup.sh"]
