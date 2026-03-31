# linux-6.1.118

```
Install linux-headers-6.1.118 only if you require to compile kernel modules

https://github.com/markbirss/linux-6.1.118/releases/tag/1

sudo dpkg -i linux-headers-6.1.118_6.1.118-17_armhf.deb
```


```
sudo apt -y update; sudo apt install build-essential libncurses-dev bison flex libssl-dev libelf-dev bc rsync dwarves lz4 git;
sudo apt -y install cpio build-essential libncurses5-dev fakeroot wget bzip2 git dpkg-dev devscripts
/usr/src/linux-6.1.118

git clone -b overlay-fs https://github.com/markbirss/linux-6.1.118.git
cd linux-6.1.118
mv .git dot_git
make rk3506_luckfox_defconfig; \
#make menuconfig
make -j$(nproc);
#make -j$(nproc) Image modules modules_install;
make -j$(nproc) Image; \
make -j$(nproc) modules_prepare; \
make -j$(nproc) modules; \
sudo make -j$(nproc) modules_install; \
sudo depmod -a; \
sudo make -j$(nproc) INSTALL_HDR_PATH=/usr/src/linux-headers-6.1.118 headers_install; \
sudo nice make -j$(nproc) bindeb-pkg


# d2832a10c5605faecb1cb55c57cc5ba4c6ed2da8dac162446040e2c470dfd8af  linux-6.1.118.tar.gz

# extract linux-6.1.118.tar.gz (677MB)
7z x linux-6.1.118.7z.001
sha256sum linux-6.1.118.tar.gz
```

error
```
sh ./scripts/package/mkdebian
dpkg-buildpackage -r"fakeroot -u" -a$(cat debian/arch)  -b -nc -uc
dpkg-buildpackage: info: source package linux-upstream
dpkg-buildpackage: info: source version 6.1.118-3
dpkg-buildpackage: info: source distribution noble
dpkg-buildpackage: info: source changed by root <root@luckfox>
dpkg-buildpackage: info: host architecture armhf
 dpkg-source --before-build .
 debian/rules binary
make KERNELRELEASE=6.1.118 ARCH=arm     KBUILD_BUILD_VERSION=3 -f ./Makefile
  UPD     include/generated/compile.h
  DTC     arch/arm/boot/dts/rk3502g-evb1-v10.dtb
  DTC     arch/arm/boot/dts/rk3506b-evb1-v10.dtb
  CALL    scripts/checksyscalls.sh
  DTC     arch/arm/boot/dts/rk3506b-test2-v10.dtb
Error: arch/arm/boot/dts/rk3502g-evb1-v10.dts:54.1-8 Label or path es8388 not found
FATAL ERROR: Syntax error parsing input tree
make[4]: *** [scripts/Makefile.lib:423: arch/arm/boot/dts/rk3502g-evb1-v10.dtb] Error 1
make[4]: *** Waiting for unfinished jobs....
Error: arch/arm/boot/dts/rk3506b-evb1-v10.dts:79.1-6 Label or path gt1x not found
FATAL ERROR: Syntax error parsing input tree
make[4]: *** [scripts/Makefile.lib:423: arch/arm/boot/dts/rk3506b-evb1-v10.dtb] Error 1
  UPD     init/utsversion-tmp.h
  CC      init/version.o
arch/arm/boot/dts/rk3506.dtsi:211.5-37: Warning (graph_endpoint): /vop@ff600000/port/endpoint@0:remote-endpoint: graph phandle is not valid
  AR      init/built-in.a
make[3]: *** [Makefile:1473: dtbs] Error 2
make[3]: *** Waiting for unfinished jobs....
  AR      built-in.a
make[2]: *** [debian/rules:7: build-arch] Error 2
dpkg-buildpackage: error: debian/rules binary subprocess returned exit status 2
make[1]: *** [scripts/Makefile.package:86: bindeb-pkg] Error 2
make: *** [Makefile:1651: bindeb-pkg] Error 2

fix

cp arch/arm/boot/dts/Makefile arch/arm/boot/dts/Makefile.original
nano arch/arm/boot/dts/Makefile
remove

dtb-$(CONFIG_ARCH_ROCKCHIP) += \
	rv1103g-38x38-ipc-v10.dtb \
	rv1103g-battery-ipc-v10.dtb \
	rv1103g-battery-ipc-v11.dtb \
	rv1103g-evb-mcu-display-v11.dtb \
	rv1103g-evb-v10.dtb \
	rv1103g-evb-v11.dtb \
	rv1103g-evb-v11-sii902x-bt6562hdmi.dtb \
	rv1103g-evb2-v10.dtb \
	rv1103g-rmsl311-dloc-sl-v10.dtb \
	rv1103g-scaner-v10.dtb \
	rv1106g-38x38-ipc-v10.dtb \
	rv1106g-38x38-ipc-v10-spi-nand.dtb \
	rv1106g-evb1-mcu-display-v11.dtb \
	rv1106g-evb1-mcu-display-v20.dtb \
	rv1106g-evb1-rgb-display-v11.dtb \
	rv1106g-evb1-v10.dtb \
	rv1106g-evb1-v10-dual-cam.dtb \
	rv1106g-evb1-v10-facial-gate.dtb \
	rv1106g-evb1-v10-spi-nand.dtb \
	rv1106g-evb1-v10-spi-nor.dtb \
	rv1106g-evb1-v11.dtb \
	rv1106g-evb1-v11-4k.dtb \
	rv1106g-evb1-v11-cvr.dtb \
	rv1106g-evb1-v11-cvr-dual-cam.dtb \
	rv1106g-evb1-v11-cvr-ext-dual-cam.dtb \
	rv1106g-evb1-v11-dual-cam.dtb \
	rv1106g-evb1-v11-facial-gate.dtb \
	rv1106g-evb1-v11-nofastae-spi-nand.dtb \
	rv1106g-evb1-v11-sii902x-bt11202hdmi.dtb \
	rv1106g-evb1-v11-sii902x-rgb2hdmi.dtb \
	rv1106g-evb1-v11-spi-nand-cvr.dtb \
	rv1106g-evb2-v10.dtb \
	rv1106g-evb2-v10-dual-camera.dtb \
	rv1106g-evb2-v11-emmc.dtb \
	rv1106g-evb2-v11-trailcam-emmc.dtb \
	rv1106g-evb2-v12-aov-spi-nor.dtb \
	rv1106g-evb2-v12-dual-camera-avs.dtb \
	rv1106g-evb2-v12-nofastae-emmc.dtb \
	rv1106g-evb2-v12-nofastae-spi-nand.dtb \
	rv1106g-evb2-v12-nofastae-spi-nor.dtb \
	rv1106g-evb2-v12-spi-nand-tb.dtb \
	rv1106g-evb2-v12-wakeup.dtb \
	rv1106g-smart-door-lock-rmsl-v10.dtb \
	rv1106g-smart-door-lock-rmsl-v12.dtb \
	rv1106g-uvc-demo-v10.dtb \
	rv1106g-uvc-demo-v10-spi-nor.dtb \
	rv1108-elgin-r1.dtb \
	rv1108-evb.dtb \
	rv1126-evb-ddr3-v10.dtb \
	rv1126-evb-ddr3-v12.dtb \
	rv1126-evb-ddr3-v12-spi-nand.dtb \
	rv1126-evb-ddr3-v12-spi-nor.dtb \
	rv1126-evb-ddr3-v13.dtb \
	rk3036-evb.dtb \
	rk3036-evb1-ddr3-v10.dtb \
	rk3036-kylin.dtb \
	rk3066a-bqcurie2.dtb \
	rk3066a-marsboard.dtb \
	rk3066a-mk808.dtb \
	rk3066a-rayeager.dtb \
	rk3126c-evb-ddr3-v10-linux.dtb \
	rk3126c-evb-ddr3-v10-linux-slc.dtb \
	rk3128-evb-ddr3-v10-linux.dtb \
	rk3128-evb-ddr3-v10-linux-spi-nand.dtb \
	rk3188-bqedison2qc.dtb \
	rk3188-px3-evb.dtb \
	rk3188-radxarock.dtb \
	rk3228-evb.dtb \
	rk3229-evb.dtb \
	rk3229-xms6.dtb \
	rk3288-evb-act8846.dtb \
	rk3288-evb-rk808.dtb \
	rk3288-firefly-beta.dtb \
	rk3288-firefly.dtb \
	rk3288-firefly-reload.dtb \
	rk3288-miqi.dtb \
	rk3288-phycore-rdk.dtb \
	rk3288-popmetal.dtb \
	rk3288-r89.dtb \
	rk3288-rock2-square.dtb \
	rk3288-rock-pi-n8.dtb \
	rk3288-tinker.dtb \
	rk3288-tinker-s.dtb \
	rk3288-veyron-brain.dtb \
	rk3288-veyron-fievel.dtb \
	rk3288-veyron-jaq.dtb \
	rk3288-veyron-jerry.dtb \
	rk3288-veyron-mickey.dtb \
	rk3288-veyron-mighty.dtb \
	rk3288-veyron-minnie.dtb \
	rk3288-veyron-pinky.dtb \
	rk3288-veyron-speedy.dtb \
	rk3288-veyron-tiger.dtb \
	rk3288-vyasa.dtb \
	rk3308-evb-audio-v10-amp-display-rgb-aarch32.dtb \
	rk3308-evb-audio-v10-display-rgb-aarch32.dtb \
	rk3308-evb-audio-v11-display-rgb-aarch32.dtb \
	rk3308bs-evb-amic-v11-aarch32.dtb \
	rk3308bs-evb-dmic-pdm-v11-aarch32.dtb \
	rk3308bs-evb-mipi-display-v11-aarch32.dtb \
	rk3308hs-voice-module-board-v10-aarch32.dtb \
	rk3502g-evb1-v10.dtb \
	rk3503g-evb1-v10.dtb \
	rk3506b-evb1-v10.dtb \
	rk3506b-test2-v10.dtb \
	rk3506g-demo-display-control.dtb \
	rk3506g-evb1-v10.dtb \
	rk3506g-evb1-v10-amp.dtb \
	rk3506g-evb1-v10-dsmc-lb-slave.dtb \
	rk3506g-evb1-v10-dsmc-master.dtb \
	rk3506g-evb1-v10-flexbus-adc-dac.dtb \
	rk3506g-evb1-v10-mcu-k350c4516t.dtb \
	rk3506g-evb1-v10-rgb-Q7050ITH2641AA1T.dtb \
	rk3506g-evb1-v10-sii9022-bt1120-to-hdmi.dtb \
	rk3506g-evb1-v10-sii9022-rgb2hdmi.dtb \
	rk3506g-evb2-v10.dtb \
	rk3506g-iotest-v10.dtb \
	rk3506g-iotest-v10-pdm.dtb \
	rk3506g-test1-v10-audio.dtb \
	rk3518-evb1-ddr4-v10.dtb \
	rk3528-demo4-ddr4-v10.dtb \
	rk3528-evb1-ddr4-v10.dtb \
	rk3528-evb2-ddr3-v10.dtb \
	rk3528-evb3-lp4x-v10.dtb \
	rk3528-evb4-ddr4-v10.dtb \
	rk3562-evb2-ddr4-v10.dtb
```

notes
```
built kernel source is required under /lib/modules/6.1.118/build

ls -lah /lib/modules/6.1.118
lrwxr-xr-x 1 root root   24 Mar 30 19:14 build -> /home/lyra/linux-6.1.118

mkdir -p /lib/modules/6.1.118/build
```
