# Introduction

A guide for building UEFI application to interact with TPM 2.0.

# Table of Contents

- **[Prerequisites](#prerequisites)**
- **[References](#references)**
- **[Create USB Bootable with UEFI Shell (64-bit)](#create-usb-bootable-with-uefi-shell-64-bit)**
- **[Build UEFI Applications](#build-uefi-applications)**
- **[Build Custom UEFI Application](#build-custom-uefi-application)**
- **[References](#references)**
- **[License](#license)**

# Prerequisites

- A Ubuntu machine (Ubuntu 20.04.2 LTS) for cross-compilation
- A 64-bit PC with TPM 2.0 and UEFI

# Create USB Bootable with UEFI Shell (64-bit)

1. Format your media as FAT32
2. Create directory structure `/efi/boot` in your media
3. Download the UEFI shell binary ([[2]](#2)) and rename the it to `Bootx64.efi`
4. Copy the binary to `/efi/boot`
5. Insert the media to your PC and configure the UEFI to boot from it
6. Eventually, you will see the UEFI shell

# Build UEFI Applications

```
$ sudo apt install build-essential uuid-dev iasl git nasm python-is-python3

$ git clone -b edk2-stable202202 https://github.com/tianocore/edk2 ~/edk2
$ cd ~/edk2
$ git submodule update --init

# Compile build tools
$ cd ~/edk2
$ make -C BaseTools
$ . edksetup.sh BaseTools

# Setup build shell environment
$ cd ~/edk2
$ export EDK_TOOLS_PATH=~/edk2/BaseTools
$ . edksetup.sh BaseTools
```

Build Hello World:
```
$ cd ~/edk2
$ build -p MdeModulePkg/MdeModulePkg.dsc -t GCC5 -a X64 -n $(nproc)
$ ls Build/MdeModule/DEBUG_*/*/HelloWorld.efi
```

Copy the `HelloWorld.efi` to the media you created and execute it in the UEFI shell.

# Build Custom UEFI Application

For demonstration purpose, reuse the HelloWorld example and apply this [patch](patch/helloworld-example-overlay.patch).

# References

<a id="1">[1] https://github.com/tianocore/edk2/</a> <br>
<a id="2">[2] https://github.com/tianocore/edk2/blob/UDK2018/ShellBinPkg/UefiShell/X64/Shell.efi</a> <br>


# License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

