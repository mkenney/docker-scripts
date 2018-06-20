#!/bin/sh
# Required for volume mounting ssh keys for "Docker for Windows"
# credit to: https://nickjanetakis.com/blog/docker-tip-56-volume-mounting-ssh-keys-into-a-docker-container
# Usage: volume mount your ssh key directory to /tmp/.ssh

set -e

cp -R /tmp/.ssh /root/.ssh
chmod 700 /root/.ssh
chmod 644 /root/.ssh/id_rsa.pub
chmod 600 /root/.ssh/id_rsa

cp -R /tmp/.ssh /home/dev/.ssh
chmod 700 /home/dev/.ssh
chmod 644 /home/dev/.ssh/id_rsa.pub
chmod 600 /home/dev/.ssh/id_rsa

exec "$@"
