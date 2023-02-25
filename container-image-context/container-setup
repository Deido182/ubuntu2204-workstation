#!/bin/bash

# Manage persistent storage
if [ ! -d "${USER_PERS_STORAGE_DIR}" ]; then
  mkdir --parents "${USER_PERS_STORAGE_DIR}"
fi
chown root:root "${PERS_STORAGE_DIR}"
chmod 770 "${USER_PERS_STORAGE_DIR}"
chgrp "${USER_PERS_STORAGE_GROUP}" "${USER_PERS_STORAGE_DIR}"

# Finally login with the new user (as we are root, pw is not required)
su - user
