Build IoT-LAB images and packages
=================================

[Yocto Project](https://www.yoctoproject.org/) aim to develop embedded Linux images and
user-space applications that run on targeted devices like IoT-LAB Open A8 nodes. 

The process of building images and packages has been wrapped in a Makefile for easier use. This will setup you environment  if it hasn't been set before, update the configuration for your machine and build the images or packages.


System Requirements
-------------------

For Ubuntu and Debian Linux distributions

    $ sudo apt-get install gawk wget git-core diffstat unzip texinfo gcc-multilib \
        build-essential chrpath socat libsdl1.2-dev xterm

For others distributions view the [Yocto official documentation](https://www.yoctoproject.org/documentation).

Build images
---------------

To build images you can run:

    $ make build-all 
    # or one by one
    $ make iotlab-image

> Do NOT attempt to build on an encrypted partition ecryptfs limits filename length to 150 characters.

Build packages
-----------------

To build package you can run:

    $ make build-pkg-<pkg_name>

Build Linux kernel/U-boot
-----------------

    $ make build-kernel
    $ make build-uboot

Manual process
--------------
   
    $ git submodule update --init (eg. only the first time)
    $ source ./poky/oe-init-build-env build
    $ bitbake -k iotlab-image-open-a8
    $ bitbake -k <pkg_name>

Yocto and OpenEmbedded
----------------------

At the top of the directory you can find a local copy of Yocto project (eg. poky directory) and a collection of [OpenEmbedded](https://www.openembedded.org/wiki/Main_Page) layers (eg. meta-openembedded).

The build system uses sub-modules and were initialized as follows:

    $ git submodule add -b krogoth git://git.yoctoproject.org/poky
    $ git submodule add -b krogoth git://git.openembedded.org/meta-openembedded
    $ git submodule add -b krogoth git://git.yoctoproject.org/meta-java
    $ git commit -m "Add submodules meta-openembedded/poky/meta-java "

Use `git submodule status` and/or check file .gitmodules for details.
   
    $ git submodule status
    67e48693501bddb80745b9735b7b3d4d28dce9a1 meta-java (heads/krogoth)
    851a064b53dca3b14dd33eaaaca9573b1a36bf0e meta-openembedded (851a064)
    cca8dd15c8096626052f6d8d25ff1e9a606104a3 poky (yocto-2.1.2)


The sub-modules are set to allow for tracking of upstream updates
on branch krogoth, current release of yocto as of Feb. 2017. You may
update the sub-modules to use the latest upstream version using:

    $ git submodule update --remote

Should you need to update either of the sub-modules to a specific version,
simply cd to it, git pull, inspect changes, then git checkout the desired
version or tag.  Then cd .. and git commit the new sub-module(s) setup.
If switching to another branch, update .gitmodules accordingly.
