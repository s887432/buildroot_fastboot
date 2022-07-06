# buildroot_fastboot
How to build:
----------------------------------------
$ git clone https://github.com/linux4sam/buildroot-at91.git<br>
$ git clone https://github.com/linux4sam/buildroot-external-microchip.git
$ cd buildroot-external-microchip/
$ git checkout linux4sam-2021.04
$ git apply ../0000-buildroot-external-microchip-fastboot-based-linux4sam-2021.04.patch
$ cd ../buildroot-at91/
$ export BR2_EXTERNAL="../buildroot-external-microchip/"
$ ln -s YOUR_DL_PATH/dl dl
$ git checkout linux4sam-2021.04
$ make sam9x60ek_graphics_dashboard_sd_defconfig
  or
$ make sam9x60ek_graphics_dashboard_nf_defconfig
$ make

How to make and install motorcycledash app:
----------------------------------------
$ cd buildroot-at91/
$ make -C output/build/egt-egt-dashdemo/examples/
$ cp output/build/egt-egt-dashdemo/examples/motorcycledash/motorcycledash output/target/root/
$ cp output/build/egt-egt-dashdemo/examples/motorcycledash/serialize.svg output/target/root/
$ cp output/build/egt-egt-dashdemo/examples/motorcycledash/deserialize.svg output/target/root/
$ mkdir -p output/target/usr/share/egt/examples/motorcycledash
$ cp output/build/egt-egt-dashdemo/examples/motorcycledash/moto.png output/target/usr/share/egt/examples/motorcycledash/
$ make

How to program image:
----------------------------------------
SD card:
	Image path:
		buildroot-at91/output/images/sdcard.img
	Program with Etcher or sam-ba commands path:
		buildroot-external-microchip/board/microchip/sam9x60ek/sam-ba_sdcard.sh
NAND Flash:
	Image path:
		buildroot-at91/output/images/boot.bin
		buildroot-at91/output/images/at91-sam9x60eknf.dtb
		buildroot-at91/output/images/zImage
		buildroot-at91/output/images/rootfs.ubi
		buildroot-external-microchip/board/microchip/sam9x60ek/logo.bmp
	sam-ba commands path:
		buildroot-external-microchip/board/microchip/sam9x60ek/sam-ba_nand.sh

How to execute motorcycledash app:
----------------------------------------
Welcome to the Microchip Demo
sam9x60ek login: root
# /root/motorcycledash

How to enable fastboot:
----------------------------------------
Repalce the /sbin/init with the following scripts:
$ cd buildroot-at91/
$ mv output/target/sbin/init output/target/sbin/init.b
$ vim output/target/sbin/init
  #!/bin/sh
  
  echo Launch EGT
  cd /root
  ./motorcycledash > /dev/ttyS0 &
  /sbin/getty -L  console 0 vt100
$ chmod +x output/target/sbin/init
$ make

