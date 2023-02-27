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
RUN apt-get -y install mysql-client
RUN apt-get -y install socat

# The first are those considered by default when running 'search'
# The 'insecure' ones instead should not be used like this
# Anyway, without doing so you would be required to always run commands with '--tls-verify=false'
RUN echo '[registries.search]' >> /etc/containers/registries.conf
RUN echo 'registries = ["registry.access.redhat.com", "registry.redhat.io", "quay.io"]' >> /etc/containers/registries.conf
RUN echo '[registries.insecure]' >> /etc/containers/registries.conf
RUN echo 'registries = ["registry.access.redhat.com", "registry.redhat.io", "quay.io"]' >> /etc/containers/registries.conf

ENV PERS_STORAGE_DIR=/persistent-storage
ENV USER_PERS_STORAGE_DIR="${PERS_STORAGE_DIR}/user"
ENV USER_PERS_STORAGE_GROUP=user-pers-storage-group
# Used by user to freely store files
ENV USER_FREE_PERS_STORAGE_DIR="${USER_PERS_STORAGE_DIR}/free"
# Used by 'persist' command to store files preserving file system structure
ENV USER_STRUCT_PERS_STORAGE_DIR="${USER_PERS_STORAGE_DIR}/structured"
ENV USER_STRUCT_PERS_STORAGE_DIR_ROOT="${USER_STRUCT_PERS_STORAGE_DIR}/root"
ENV USER_STRUCT_PERS_FILES_LIST="${USER_STRUCT_PERS_STORAGE_DIR}/.files-list"

RUN useradd -m user && echo "user:user" | chpasswd
RUN addgroup "${USER_PERS_STORAGE_GROUP}"
RUN adduser user "${USER_PERS_STORAGE_GROUP}"
# This is to make user a 'sudoer', if strictly needed
#RUN adduser user sudo

# Change default user shell
RUN usermod --shell /bin/bash user

# Add default (useful) options for vim
RUN echo 'set cursorline' >> /home/user/.vimrc
RUN echo 'set cursorcolumn' >> /home/user/.vimrc
RUN echo 'set number' >> /home/user/.vimrc

# Command to structurally persist/unpersist user files
COPY user-persist /usr/bin/persist
RUN chmod 770 /usr/bin/persist
RUN chgrp "${USER_PERS_STORAGE_GROUP}" /usr/bin/persist
COPY user-unpersist /usr/bin/unpersist
RUN chmod 770 /usr/bin/unpersist
RUN chgrp "${USER_PERS_STORAGE_GROUP}" /usr/bin/unpersist 
# The following must be performed at runtime
COPY container-setup /.setup
ENTRYPOINT ["/bin/bash", "-c"]
CMD ["/.setup"]
