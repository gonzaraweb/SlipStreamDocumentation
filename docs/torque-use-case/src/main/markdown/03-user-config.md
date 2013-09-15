User Account Configuration
==========================

Introduction
============

Torque uses standard unix account information to authorize users to
access the Torque server, to execute jobs on the computing resources,
and to return job output to the user. Although there are several ways to
set up the batch system to achieve this, the easiest is to have a common
account configuration and common file system shared by all of the
machines in the system. In production, an LDAP server usually provides
the account configuration and an NFS server, the shared file system.

To simplify this use case, a common account configuration (including SSH
keys) will be created by using a common machine image for all of the
machines in the batch system. This allows the configuration to be done
once and to avoid having to dynamically copy SSH keys between machines
at runtime. Use of common machine images is a useful technique for
distributing a common configuration between machines in a deployment.

Creating Machine Image
======================

To create the base image (named "Cluster\_Base\_Image") with the account
configuration, a new machine image is created with the following recipe:

    #!/bin/sh -x

    # Add a group that will contain all of the users.
    GROUP=users
    groupadd ${GROUP}

    # Loop over the desired user names, creating the account
    # and setting up the ssh configuration.
    for USERNAME in "userx" "usery" "userz"; do

      # Create the user.  Make sure that the PRIMARY group is
      # 'users'.  This is needed for torque spool areas.
      useradd --create-home -g users ${USERNAME}

      # Setup various locations for the ssh configuration.
      HOME_DIR=/home/${USERNAME}
      SSH_DIR=${HOME_DIR}/.ssh
      KEY_FILE=${SSH_DIR}/id_rsa

      # Remove any existing ssh directory and create it from scratch.
      # This will also remove any ssh keys and configurations.
      if [ -e ${SSH_DIR} ]; then
        rm -Rf ${SSH_DIR}
      fi
      mkdir -p ${SSH_DIR}

      # Generate the key for this account.  (This has NO password.)
      ssh-keygen -t rsa -f ${KEY_FILE} -N ""

      # Generate the authorized key files.
      cp -f ${KEY_FILE}.pub ${SSH_DIR}/authorized_keys
      cp -f ${KEY_FILE}.pub ${SSH_DIR}/authorized_keys2

      # Ensure that all of the files have the correct owner and group.
      chown -R ${USERNAME}:${GROUP} ${SSH_DIR} 

    done

This creates three user accounts (userx, usery, and userz) along with
new SSH keys without passwords. Once the image has been created, then
any other image which depends on this will share the given account
configuration.