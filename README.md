lxc-build-tools
===============

A set of tools to help with using lxc in a build environment.

Usage
-----

### Create the initial container

On a build server, as a one time operation, you need to create your container.  

./create_container CONTAINER_NAME

This will use the template ubuntu-nopasswd, which will put ~/.ssh/id_rsa.pub in the authorized_keys of the container, as well as make the ubuntu user have nopasswd sudo access.


You then need to prepare the container how you like using ssh or lxc-console

### Snapshot the container at a start state suitable for the build

Also on the build server, as a one time operation, you need to take a snapshot using lxc-snap: 

* http://s3hh.wordpress.com/2013/05/06/introducing-lxc-snap/
* https://github.com/hallyn/lxc-snap

./snapshot_container CONTAINER_NAME 



Now, you have a container, and a snapshot, which is named CONTAINER_NAME_0  (an appended '_0' is done automatically by lxc-snap. You can verify this by running `lxc-snap -l CONTAINER_NAME)

### In the beginning of your build script
./reset_container CONTAINER_NAME SNAPSHOT_NAME

After this, you can ssh to your container for some sort of build automation (like perhaps using knife bootstrap to install chef and test a chef script)

Tips
----

If you are creating the container over and over, you may need to do the following to not go crazy dealing with your known_hosts file:

```
Host CONTAINER_NAME
 UserKnownHostsFile=/dev/null
 StrictHostKeyChecking no
```

Also, if you are creating the root container over and over, you can use `./destroy_container CONTAINER_NAME` just to run lxc-stop and lxc-destroy for you with a little less typing.
