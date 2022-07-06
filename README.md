# buildroot_fastboot
How to build:
----------------------------------------
$ git clone https://github.com/linux4sam/buildroot-at91.git<br>
$ git clone https://github.com/linux4sam/buildroot-external-microchip.git<br>
$ cd buildroot-external-microchip/<br><br>
$ git checkout linux4sam-2021.04<br>
$ git apply ../0000-buildroot-external-microchip-fastboot-based-linux4sam-2021.04.patch<br>
$ cd ../buildroot-at91/<br>
$ export BR2_EXTERNAL="../buildroot-external-microchip/"<br>
$ ln -s YOUR_DL_PATH/dl dl<br>
$ git checkout linux4sam-2021.04<br>
$ make sam9x60ek_graphics_dashboard_sd_defconfig<br>
  or<br>
$ make sam9x60ek_graphics_dashboard_nf_defconfig<br>
$ make<br>

How to make and install motorcycledash app:<br>
----------------------------------------<br>
$ cd buildroot-at91/<br>
$ make -C output/build/egt-egt-dashdemo/examples/<br>
$ cp output/build/egt-egt-dashdemo/examples/motorcycledash/motorcycledash output/target/root/<br>
$ cp output/build/egt-egt-dashdemo/examples/motorcycledash/serialize.svg output/target/root/<br>
$ cp output/build/egt-egt-dashdemo/examples/motorcycledash/deserialize.svg output/target/root/<br>
$ mkdir -p output/target/usr/share/egt/examples/motorcycledash<br>
$ cp output/build/egt-egt-dashdemo/examples/motorcycledash/moto.png output/target/usr/share/egt/examples/motorcycledash/<br>
$ make<br>

How to program image:<br>
----------------------------------------<br>
SD card:<br>
	Image path:<br>
		buildroot-at91/output/images/sdcard.img<br>
	Program with Etcher or sam-ba commands path:<br>
		buildroot-external-microchip/board/microchip/sam9x60ek/sam-ba_sdcard.sh<br>
NAND Flash:<br>
	Image path:<br>
		buildroot-at91/output/images/boot.bin<br>
		buildroot-at91/output/images/at91-sam9x60eknf.dtb<br>
		buildroot-at91/output/images/zImage<br>
		buildroot-at91/output/images/rootfs.ubi<br>
		buildroot-external-microchip/board/microchip/sam9x60ek/logo.bmp<br>
	sam-ba commands path:<br>
		buildroot-external-microchip/board/microchip/sam9x60ek/sam-ba_nand.sh<br>

How to execute motorcycledash app:<br>
----------------------------------------<br>
Welcome to the Microchip Demo<br>
sam9x60ek login: root<br>
# /root/motorcycledash<br>

How to enable fastboot:<br>
----------------------------------------<br>
Repalce the /sbin/init with the following scripts:<br>
$ cd buildroot-at91/<br>
$ mv output/target/sbin/init output/target/sbin/init.b<br>
$ vim output/target/sbin/init<br>
  #!/bin/sh<br>
  
  echo Launch EGT<br>
  cd /root<br>
  ./motorcycledash > /dev/ttyS0 &<br>
  /sbin/getty -L  console 0 vt100<br>
$ chmod +x output/target/sbin/init<br>
$ make<br>

