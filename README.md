# Home Server

## Single-Board Computer

### [Renegade ROC-RK3328-CC](https://libre.computer/products/roc-rk3328-cc/) 4GB

#### [NixOS](https://wiki.nixos.org/wiki/NixOS_on_ARM/Libre_Computer_ROC-RK3328-CC)

[Download from Hydra builds](https://hydra.nixos.org/job/nixpkgs/trunk/ubootRock64.aarch64-linux).

I am using [this build](https://hydra.nixos.org/build/266591732).

```bash
wget https://hydra.nixos.org/build/266591732/download/1/u-boot.itb
wget https://hydra.nixos.org/build/266591732/download/2/idbloader.img
wget https://hydra.nixos.org/build/266591732/download/3/u-boot-rockchip.bin

diskutil list

diskutil unmount /dev/disk5s1

dd if=idbloader.img of=/dev/disk5s1 conv=fsync,notrunc bs=512 seek=64
dd if=u-boot.itb of=/dev/disk5s1 conv=fsync,notrunc bs=512 seek=16384

diskutil eject /dev/disk5s1
```

After booting it, it got stuck in the Orange Pi initial screen.

Using Balena Etcher I discovered that the SD card is too small for the image.

### [Orange Pi 3B](http://www.orangepi.org/html/hardWare/computerAndMicrocontrollers/details/Orange-Pi-3B.html) 4GB

[Ubuntu Server](https://ubuntu.com/download/server/arm)
