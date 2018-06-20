# `run-as-user`

By default, force the `dev` user's `uid` and `gid` to match the user/group that owns the project directory (`/src`) and run a command as that user. If a ~/.ssh directory has been mounted into the `dev` user's home directory (`/home/dev`) and it's not owned by root, then that `uid` and `gid` will be used instead in order to use public key authentication.

If the project directory is owned by `root` then files will be written out as `root`...

# `composer-wrapper`

Credit goes to https://github.com/graze/docker-composer/blob/master/php-7.0/composer-wrapper

* Loop over each argument and append the argument if the command matches one we need to use `--ignore-platform-reqs` with. Found using the following search: https://github.com/composer/composer/search?q=ignore-platform-reqs+path%3Asrc%2FComposer%2FCommand%2F Uses `set` to update the arguments, see https://www.gnu.org/software/bash/manual/html_node/The-Set-Builtin.html.

# `set-ssh-key-perms`

Credit goes to https://nickjanetakis.com/blog/docker-tip-56-volume-mounting-ssh-keys-into-a-docker-container

Initial use case is for users of "Docker for Windows". Due to file permission differences with the Windows OS and Linux, you need to adjust them inside the container before use.

* Copy the ssh keys mounted to `/tmp/` to `/home/dev/` and `/root/`. Ensuring the original are not modified.
* Set permissions on `.ssh/` directories. Ensuring the permissions are correct.
