# linux-6.1.118

```
sudo apt -y update; sudo apt install build-essential libncurses-dev bison flex libssl-dev libelf-dev bc rsync dwarves lz4 git;

/usr/src/linux-6.1.118

git clone -b overlay-fs https://github.com/markbirss/linux-6.1.118.git
cd linux-6.1.118
mv .git dot_git
make rk3506_defconfig; \
#make menuconfig
make -j4;
#make -j4 Image modules modules_install;
make -j4 Image; \
make -j4 modules_prepare; \
make -j4 modules; \
sudo make -j4 modules_install; \
sudo depmod -a; \
sudo make -j4 INSTALL_HDR_PATH=/usr/src/linux-headers-6.1.118 headers_install; \
sudo nice make -j$(nproc) bindeb-pkg
```
