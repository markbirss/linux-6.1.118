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
sudo make -j$(nproc) modules_install; \
sudo depmod -a; \
sudo make -j$(nproc) INSTALL_HDR_PATH=/usr/src/linux-headers-6.1.118 headers_install; \
sudo nice make -j$(nproc) bindeb-pkg
```
