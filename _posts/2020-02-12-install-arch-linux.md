---
title: "Arch linux 설치하기"
excerpt: "아치 리눅스 설치 및 기본 설정"
date: 2020-02-12 19:25:06 -0400
categories: 
  -linux
tags:
  -arch
  -linux
  -arch-linux

last_modified_at: 2020-02-12 22:35:10
---

# 아치 리눅스 설치하기

last modified at: {{ page.last_modified_at }}

나중에 재설치할 경우를 대비해서 작성

reference: <a href="https://withjeon.com/2017/11/07/arch-linux-install-guide/"> https://withjeon.com/2017/11/07/arch-linux-install-guide/</a>

## iso 다운로드 및 설치준비

1. 다운로드

[https://www.archlinux.org/download/](https://www.archlinux.org/download/)

이곳에서 서버 부담을 줄이기 위해 토렌트를 이용해서 다운로드 받는다.

2. 설치준비

`lsblk`

로 USB 디바이스 확인한 다음

`sudo dd bs=4M if=/directory/to/archlinux.iso of=/dev/sdb status=progess && sync`

로 구워줌

## 설치

USB를 꽂고 부팅 -> bios 진입 -> arch linux 선택

1. 파티션

`lsblk`

sda를 두 개로 쪼갬

`cfdisk /dev/sda`

```
new
512M
type
efi system

그 아래의 free space

new
default대로(남은 용량 전부)
type
linux filesystem
write
quit
```

2. 포멧

```
mkfs.vfat -F32 /dev/sda1
mkfs.ext4 /dev/sda2
```

3. 마운트

```
mount /dev/sda2 /mnt
mkdir /mnt/boot
mount /dev/sda1 /mnt/boot
```

4. 인터넷, 미러리스트

```
wifi-menu
와이파이 연결
```

`nano -w /etc/pacman.d/mirrorlist`
미러리스트 제일 위에 한국 내의 서버를 추가
```
Server = http://ftp.lanet.kr/pub/archlinux/$repo/os/$arch
Server = https://ftp.lanet.kr/pub/archlinux/$repo/os/$arch
```

5. pacstrap

`pacstrap /mnt base linux linux-firmware base-devel`

6. genfstab

`genfstab -U /mnt >> /mnt/etc/fstab`

## 시스템 설정

1. 루트 진입

`arch-chroot /mnt`

2. 패스워드 변경

`#passwd`

3. 로케일 설정

`#nano /etc/locale.gen`

`en_US.UTF-8` 만 주석 해제

`#locale-gen`

4. 언어 설정

```
#echo LANG=en_US.UTF-8 > /etc/locale.conf
#export LANG=en_US>UTF-8
```

5. 호스트 네임 설정

`#echo hostname > /etc/hostname`

6. 타임존

`#ln -sf /usr/share/zoneinfo/Asia/Seoul /etc/localtime`

`#hwclock --systohc --utc`

7. 새 계정 추가

`#useradd -m -g users -G wheel myname`

`#passwd myname`

`#EDITOR=nano visudo`

/etc/sudoers 파일을 수정

`%wheel All=` 부분을 주석해제

8. 팩맨 설정

`nano /etc/pacman.conf`

Misc options에서 Color 주석해제 + ILoveCandy 추가

9. 부트로더

```
#pacman -Syu
#pacman -S grub efibootmgr

#grub-install --target=x86_64-eft --efi-directory=/boot --bootloader-id=arch --recheck
#grub-mkconfig -o /boot/grub/grub.cfg
```

10. 네트워크

```
#pacman -S networkmanager
#systemctl enable NetwrokManager.service
```

11. 재부팅

`#exit`

`#reboot`

USB를 제거하면 아치 리눅스로 부팅된다


## 데스크톱 환경 설치

설정한 id로 로그인

1. 인터넷

`$nmtui`로 설정

2. 그놈 설치

```
$sudo pacman -Syu
$sudo pacman -S xorg-server gnome
$sudo systemctl enable gdm
```

3. 재부팅

`$sudo pacman -S intel-ucode`
`$grub-mkconfig -o /boot/grub/grub.cfg`

## 필수 소프트웨어 설치

1. 브라우저

```
$sudo pacman -Syu
$sudo pacman -S firefox
$sudo pacman -S chromium
```

2. AUR 매니저

aur.archlinux.org에 접속 -> yay 검색 -> snapshot 다운로드

```
$tar -xvf yay
$cd yay
$makepkg -sic
```

3. 한글 사용하기

yay를 사용해서 나눔 폰트를 설치

`$yay ttf-nanum`

```
$sudo pacman -S ibus ibus-hangul
```

ibus-preferences를 클릭 -> input method -> Korean - Hangul을 찾아 Add

Setting -> Region & Language -> Input Sources -> others -> Korean(hangul)

4. IDE

```
$sudo pacman -S vim nano
$sudo pacman -S code
$yay intellij-idea-ultimate-edition
```

git 설치
```
$sudo pacman -S git
$yay smartgit
```


5. 멀티미디어

```
$sudo pacman -S transmission-gtk
$sudo pacman -S vlc
```

