#!/bin/sh

##############################################################################
# If a PUID/PGID enviroment variable exists, use those values for the `uid`
# and `gid` when executing scripts, otherwise change the dev user's uid and
# gid to match the user that owns the project directory and run a command as
# that user. If a ~/.ssh directory exists and it's not owned by root then
# switch and run as that user instead in order to take advantage of public key
# authentication.
##############################################################################

stat_dir="/src"
if [ -d "/home/dev/.ssh" ] && [ "0" != "$(stat -c '%g' /home/dev/.ssh)" ] && [ "0" != "$(stat -c '%u' /home/dev/.ssh)" ]; then
    stat_dir="/home/dev/.ssh"
fi

# if the PUID environment variable exists, assume that is the preferred user id,
# otherwise use the $stat_dir
if [ "" != "$PUID" ]; then
    uid=$PUID
else
    uid=$(stat -c '%u' $stat_dir)
fi

# if the PGID environment variable exists, assume that is the preferred group id,
# otherwise use the $stat_dir
if [ "" != "$PGID" ]; then
    gid=$PGID
else
    gid=$(stat -c '%g' $stat_dir)
fi

# Ensure the correct user id is available in the sudoers file (if the
# specified UID already existed in the image)
id -nu $uid > /dev/null 2>&1
if [ 0 -eq $? ]; then
    echo "$(id -nu $uid) ALL=(ALL:ALL) NOPASSWD: ALL" >> /etc/sudoers
fi

# update the dev user with the specified UID/GID values
groupmod -g $gid -o dev > /dev/null 2>&1
usermod -u $uid -o dev > /dev/null 2>&1
chown -R $uid:$gid ~dev/ > /dev/null 2>&1

# prevent circular calls to this script (legacy support)
cmd="$@"
cmda=`expr "x$cmd" : "x.\{0\}\(.\{0,12\}\)"`
cmdb=`expr "x$cmd" : "x.\{1\}\(.\{1,12\}\)"`

while [ "/run-as-user" = "${cmda}" ] || [ "/run-as-user" = "${cmdb}" ]; do
    shift
    cmd="$@"
done

sudo -i -u dev $@
