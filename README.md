# qemu-systemc-docker

Copy systemc-2.3.3.tar.gz file to this folder.

Then run:

`docker build -t qemu-systemc:2020.2 .`

After installation, launch a container with (including X forwarding):

`docker run -ti -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix -v $HOME/.Xauthority:/home/xilinx/.Xauthority qemu-systemc:2020.2`
