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

#### [Orange Pi Ubuntu Server](http://www.orangepi.org/html/hardWare/computerAndMicrocontrollers/service-and-support/Orange-Pi-3B.html) 

Download Ubuntu Server image in this [link](http://www.orangepi.org/html/hardWare/computerAndMicrocontrollers/service-and-support/Orange-Pi-3B.html)

```bash
nix-shell -p p7zip --run "7za x Orangepi3b_1.0.6_ubuntu_jammy_server_linux5.10.160.7z"

# find the sd card identifier, it is a internal, physical
diskutil list

diskutil unmount /dev/disk5s1

sudo dd if=Orangepi3b_1.0.6_ubuntu_jammy_server_linux5.10.160.img of=/dev/disk5s1

diskutil eject /dev/disk5s1
```

#### [Ubuntu Server](https://ubuntu.com/download/server/arm) :red_circle

```bash
wget https://cdimage.ubuntu.com/releases/24.04/release/ubuntu-24.04-live-server-arm64.iso

# find the sd card identifier, it is a internal, physical
diskutil list

diskutil unmount /dev/disk5s1

sudo dd if=ubuntu-24.04-live-server-arm64.iso of=/dev/disk5s1

diskutil eject /dev/disk5s1
```

After booting it, it got stuck in the Orange Pi initial screen.

##### Using Balena Etcher

After booting it, it got stuck in the Orange Pi initial screen.

#### [NixOS](https://nixos.wiki/wiki/NixOS_on_ARM#Installation) :red_circle

I tried [unstable (LTS kernel)](https://hydra.nixos.org/build/265833086) but it didn't work:

```bash
wget https://hydra.nixos.org/build/265833086/download/1/nixos-sd-image-23.11.7870.205fd4226592-aarch64-linux.img.zst
# decompress the image
nix-shell -p zstd --run "unzstd nixos-sd-image-23.11.7870.205fd4226592-aarch64-linux.img.zst"

# find the sd card identifier, it is a internal, physical
diskutil list

diskutil unmount /dev/disk5s1

sudo dd if=nixos-sd-image-23.11.7870.205fd4226592-aarch64-linux.img of=/dev/disk5s1

diskutil eject /dev/disk5s1
```

After booting it, it got stuck in the Orange Pi initial screen.

## Game Console

### Miyoo Mini+ V2

Since it is Linux and has SSH, I consider it a server.

```bash
nix-shell -p wget --run "wget https://github.com/OnionUI/Onion/releases/download/v4.3.1-1/Onion-v4.3.1-1.zip"

unzip Onion-v4.3.1-1.zip -d ./Onion-v4.3.1-1

diskutil unmount /dev/disk5s1

sudo diskutil eraseDisk FAT32 ONION MBRFormat /dev/disk5

cp --recursive ./Onion-v4.3.1-1/* /Volumes/ONION/
# For some reasom the above command did not copied the .tmp_update directory
cp --recursive ./Onion-v4.3.1-1/.tmp_update/ /Volumes/ONION/.tmp_update/

diskutil eject /dev/disk5s1
```
