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