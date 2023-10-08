# qemu-systemc-docker

Copy systemc-2.3.4.tar.gz file to this folder.

Then run:

`docker build -t qemu-systemc:2022.2 .`

After installation, launch a container with (including X forwarding):

`docker run -ti -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix -v $HOME/.Xauthority:/home/xilinx/.Xauthority qemu-systemc:2022.2`

# Running a normal simulation

First, either bind mount a directory with the boot images generated by PetaLinux:

`docker run -ti -v <petalinux_root>/images:/home/xilinx/images qemu-systemc:2022.2`

or copy the image directory into the (existing) container:

`docker cp <petalinux_root>/images <container>:/home/xilinx/images`

Then, inside the container, setup permissions and launch QEMU:

      sudo chown -R xilinx.xilinx images
      ./qemu-boot

# Running a co-simulation

Copy or bind mount a boot image that contains a `zynqmp-qemu-multiarch-arm.cosim.dtb` co-simulation DTB. Then, launch the QEMU co-simulation instance insider the container with:

     ./qemu-boot --cosim [<quantum>]

The simulation quantum is optional and defaults to 1000000.

Then switch to another host terminal, and launch a second shell in your container:

`docker exec -it <container> /bin/bash`

and run the SystemC side of the co-simulation using the same `<quantum>` as given on the QEMU side:

    cd systemctlm-cosim-demo
    ./zynqmp_demo unix:../tmp/qemu-rport-_amba@0_cosim@0 <quantum>

