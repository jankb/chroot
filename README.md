# chroot aka poor mans Docker ;)
A bare minimum of what you need to set up a chroot environment.
Beware, this will not create a safe 'jail' where one can test potentially 
malicious software or code.

### What you need
* A host OS, I used Ubuntu 22.04 LTS.
* A guest filesystem, I used [Alpine Linux 3.19.1 'mini root filesystem'](https://alpinelinux.org/downloads/)

### How to
* Create a folder on the host system, e.g `minifs`
 Download the tarball from Alpine Linux for your host architecture into the folder and extract it `$ tar zxvf alpine-minirootfs-3.19.1_x86_64.tar.gz`
* When in the `minifs/` folder mount system folders needed for kernel:
```
sudo mount --bind /proc ./proc/
sudo mount --bind /sys ./sys/
sudo mount --bind /dev ./dev/
```
* Copy the `resolv.conf`from host to `/etc` in `/minifs`
```
cp /etc/resolv.conf ./etc
```
* Run chroot to switch to Alpine Linux filesystem (guest)
```
sudo chroot . bin/ash -l
```
* This should give you root in the guest filesystem
```
/#
```
* Try to update the packages to see that it works.
```
apk update
```


### Stuff to try
* Run graphical apps in chroot, e.g xeyes
   * In guest (chroot environment) install xeyes
```
apk add xeyes
```
   * In host, allow local connections
```
xhost +local:
```