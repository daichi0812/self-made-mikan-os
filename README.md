# 概要
ゼロからのOS自作入門（ミカンOS）

ここでは使用するコマンドを列挙ししていく
---
## 環境構築
windowsにwslをインストールしてwslでqemuを使う（エミュレーターをつかう）
管理者権限のPoweShellにて
```
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```
```
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```
```
wsl --set-default-version 2
```

# 第1章 PCの仕組みとハローワールド
Ubuntu(WSL)にて
```
sudo apt install qemu-utils
```
```
qemu-img create -f raw disk.img 200M
```
```
sudo apt install dosfstools
```
```
 mkfs.fat -n 'MIKAN OS' -s 2 -f 2 -R 32 -F 32 disk.img
 ```
 ```
 mkdir -p mnt
 ```
 ```
 sudo mount -o loop disk.img mnt
 ```
 ```
 sudo mkdir -p mnt/EFI/BOOT
 ```
 ```
 sudo cp BOOTX64.EFI mnt/EFI/BOOT/BOOTX64.EFI
 ```
 ```
 sudo umount mnt
 ```
 ```
 sudo apt install qemu-system-x86
 ```
 ```
 sudo apt install ovmf
 ```
 ```
 mkdir -p $HOME/osbook/devenv
 ```
 ```
 cp /usr/share/OVMF/OVMF_CODE_4M.fd $HOME/osbook/devenv/
 ```
 ```
 cp /usr/share/OVMF/OVMF_VARS_4M.fd $HOME/osbook/devenv/
 ```
 ```
 qemu-system-x86_64 \
    -k ja \
    -drive if=pflash,file=$HOME/osbook/devenv/OVMF_CODE_4M.fd \
    -drive if=pflash,file=$HOME/osbook/devenv/OVMF_VARS_4M.fd \
    -hda disk.img
```
```
cd $HOME/osbook/day01/c
```
```
sudo apt install clang
```
```
clang -target x86_64-pc-win32-coff \
    -mno-red-zone -fno-stack-protector -fshort-wchar -Wall -c hello.c
```
```
sudo apt install lld
```
```
lld-link /subsystem:efi_application /entry:EfiMain /out:hello.efi hello.o
```
```
$HOME/osbook/devenv/run_qemu.sh hello.efi
```