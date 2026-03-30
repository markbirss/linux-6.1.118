# linux-6.1.118

```
sudo apt -y update; sudo apt install build-essential libncurses-dev bison flex libssl-dev libelf-dev bc rsync dwarves lz4 git;
sudo apt -y install cpio build-essential libncurses5-dev fakeroot wget bzip2 git dpkg-dev devscripts
/usr/src/linux-6.1.118

git clone -b overlay-fs https://github.com/markbirss/linux-6.1.118.git
cd linux-6.1.118
mv .git dot_git
make rk3506_defconfig; \
#make menuconfig
make -j$(nproc);
#make -j$(nproc) Image modules modules_install;
make -j$(nproc) Image; \
make -j$(nproc) modules_prepare; \
make -j$(nproc) modules; \
#sudo make -j$(nproc) modules_install; \
#sudo depmod -a; \
sudo make -j$(nproc) INSTALL_HDR_PATH=/usr/src/linux-headers-6.1.118 headers_install; \
sudo nice make -j$(nproc) bindeb-pkg
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


```

notes
```
built kernel source is required under /lib/modules/6.1.118/build

ls -lah /lib/modules/6.1.118
lrwxr-xr-x 1 root root   24 Mar 30 19:14 build -> /home/lyra/linux-6.1.118

mkdir -p /lib/modules/6.1.118/build
```
