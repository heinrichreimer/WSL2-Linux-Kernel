# linux-wsl2

Linux kernel for Microsoft Windows Subsystem for Linux 2 (WSL2).
Modified for usage with Ceph, NFS 4.1.

# Build and deploy

Open WSL2 and install dependencies:
```shell script
sudo apt install flex bison libelf-dev libssl-dev crudini
```

Prepare scripts:
```shell script
make prepare scripts
```

Configure the kernel:
```shell script
make menuconfig
```

Build the kernel:
```shell script
make
```

Install kernel modules:
```shell script
sudo make modules_install
```

Copy kernel image:
```shell script
mkdir -p /mnt/c/Sdk/Wsl
cp arch/x86/boot/bzImage /mnt/c/Sdk/Wsl/kernel
```

Configure Windows to use the custom kernel:
```shell script
crudini --set "$(wslpath "$(wslvar USERPROFILE)")/.wslconfig" wsl2 kernel "$(wslpath -w "/mnt/c/Sdk/Wsl/kernel" | sed 's/\\/\\\\/g')"
```

Now exit the WSL2 shell and open Powershell as an administrator.

Restart WSL2:
```shell script
wsl --shutdown
Restart-Service LxssManager
```

You can now run `wsl` with the custom kernel.
The kernel runs all Linux dstributions, so you only have to change the kernel once.

_Inspired by https://wsl.dev/wsl2-kernel-zfs/._
