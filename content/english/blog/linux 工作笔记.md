---
title: linux 工作笔记
date: 2022-04-04T05:00:00Z
image: /images/image-placeholder.png
categories:
  - 技术
tags:
  - linux
---
<!--more-->

{{< toc >}}
## CentOS

## [](https://littleriver.cc/linux#heading-httpsdocslittleriverccv1referenceslinuxcentos "Permalink")[​](https://docs.littleriver.cc/v1/references/linux#centos)

## [](https://littleriver.cc/linux#heading-ubuntuhttpsdocslittleriverccv1referenceslinuxubuntu "Permalink")Ubuntu[​](https://docs.littleriver.cc/v1/references/linux#ubuntu)

## [](https://littleriver.cc/linux#heading-httpsdocslittleriverccv1referenceslinuxe5b8b8e794a8e591bde4bba4 "Permalink")常用命令[​](https://docs.littleriver.cc/v1/references/linux#%E5%B8%B8%E7%94%A8%E5%91%BD%E4%BB%A4)

### [](https://littleriver.cc/linux#heading-httpsdocslittleriverccv1referenceslinuxe69fa5e79c8be7abafe58fa3e8bf9ee68ea5e695b0 "Permalink")查看端口连接数[​](https://docs.littleriver.cc/v1/references/linux#%E6%9F%A5%E7%9C%8B%E7%AB%AF%E5%8F%A3%E8%BF%9E%E6%8E%A5%E6%95%B0)

COPY

```shell
netstat -nat|grep -i "80"|wc -l
```

## [](https://littleriver.cc/linux#heading-httpsdocslittleriverccv1referenceslinuxe8bdafe4bbb6e4bdbfe794a8 "Permalink")软件使用[​](https://docs.littleriver.cc/v1/references/linux#%E8%BD%AF%E4%BB%B6%E4%BD%BF%E7%94%A8)

### [](https://littleriver.cc/linux#heading-screen-httpsdocslittleriverccv1referenceslinuxscreen-e5908ee58fb0e8bf90e8a18c "Permalink")Screen 后台运行[​](https://docs.littleriver.cc/v1/references/linux#screen-%E5%90%8E%E5%8F%B0%E8%BF%90%E8%A1%8C)

Linux 上Screen的使用

COPY

```shell
# 安装screen
sudo yum install -y screen

#进入工程目录
cd /alidata/redis-shake-v2.0.3

# 新建窗口
screen -S redis-shake

# 执行命令
screen ./redis-shake.linux -conf=redis-shake.conf -type=sync

#分离会话
ctrl+a

#列出screen
screen -ls

#进入screen
screen -r
```

> 参考地址： [https://blog.csdn.net/hejunqing14/article/details/50338161](https://blog.csdn.net/hejunqing14/article/details/50338161)

### [](https://littleriver.cc/linux#heading-httpsdocslittleriverccv1referenceslinuxe7a381e79b98e68c82e8bdbde5b9b6e6a0bce5bc8fe58c96 "Permalink")磁盘挂载并格式化[​](https://docs.littleriver.cc/v1/references/linux#%E7%A3%81%E7%9B%98%E6%8C%82%E8%BD%BD%E5%B9%B6%E6%A0%BC%E5%BC%8F%E5%8C%96)

[](http://linuxvmdatadiskautoinitialize.sh/)**[LinuxVMDataDiskAutoInitialize.sh](http://linuxvmdatadiskautoinitialize.sh/)** **文件脚本**

COPY

```shell
#!/bin/bash

export PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin:/root/bin
clear
check_lvm=`lsblk | grep -w "/" | grep lvm`
if [ -n "${check_lvm}" ];then
  echo -e -n "\033[31m\nThis script do not support LVM.\033[0m"
  exit
fi
echo -e "\n\033[36mStep 1: Initializing script and check root privilege\033[0m"
if [ "$(id -u)" = "0" ];then  
    echo -e "\033[33mIs running, please wait!\033[0m"
    yum -y install e4fsprogs > /dev/null 2>&1
    echo -e "\033[32mSuccess, the script is ready to be installed!\033[0m"
else
    echo -e "\033[31mError, this script must be run as root!\n\033[0m"
    exit 1
fi
echo -e "\n\033[36mStep 2: Show all active disks:\033[0m"
fdisk -l 2>/dev/null | grep -o "Disk /dev/.*d[a-z]" | grep -v "/dev/vda"
echo -e -n "\n\033[36mStep 3: Please choose the disk(e.g.: /dev/vdb and q to quit):\033[0m"
read Disk
if [ $Disk == q ];then
    exit
fi
OS_mount_disk=`lsblk -nl | grep -w "/" | grep part | awk '{print $1}' | tr -d '0-9'`
if [ -z "${OS_mount_disk}" ];then
  echo -e -n "\033[31m\nCan not get OS_mount_disk and exit.\033[0m"
  exit
fi
until fdisk -l 2>/dev/null | grep -o "Disk /dev/.*d[a-z]" | grep -v "/dev/${OS_mount_disk}" | grep -w "Disk $Disk" &>/dev/null;do
echo -e -n "\033[31mOops, something went wrong, please try again (e.g.: /dev/vdb or q to quit):\033[0m"
    read Disk
    if [ $Disk == q ];then
        exit
    fi
done
while mount | grep "$Disk" > /dev/null 2>&1;do
    echo -e "\033[31m\nYour disk has been mounted:\033[0m"
    mount | grep "$Disk"
    echo -e -n "\033[31m\nForce uninstalling? [y/n]:\033[0m"
    read Umount
    until [ $Umount == y -o $Umount == n ];do
        echo -e -n "\033[31mOops, something went wrong, please try again [y/n]:\033[0m"
        read Umount
    done
    if [ $Umount == n ];then
        exit
    else
        echo -e "\033[33mIs running, please wait!\033[0m"
        for i in `mount | grep "$Disk" | awk '{print $3}'`;do
            fuser -km $i >/dev/null
            umount $i >/dev/null
            dev_name=$(cat /etc/fstab | grep "${Disk}" | awk '{ print $1 }')
            if [ -n "$dev_name" ]; then
                device=$(echo "${Disk}" | sed 's;/;\\\/;g')
                [ -n "${device}" ] && sed -i -e "/^$device/d" /etc/fstab
            else
                UUID=$(blkid -s UUID "${Disk}1" | awk '{ print $2 }' | tr -d '"')
                [ -n "${UUID}" ] && sed -i -e "/^${UUID}/d" /etc/fstab
            fi
            sleep 2
        done
        echo -e "\033[32mSuccess, the disk is unloaded!\033[0m"
    fi
    echo -e -n "\n\033[36mReady to begin to format the disk? [y/n]:\033[0m"
    read Choice
    until [ $Choice == y -o $Choice == n ];do
        echo -e -n "\033[31mOops, something went wrong, please try again [y/n]:\033[0m"
        read Choice
    done
    if [ $Choice == n ];then
        exit
    else
        echo -e "\033[33mIs running, please wait!\033[0m"
        dd if=/dev/zero of=$Disk bs=512 count=1 &>/dev/null
        sleep 2
    sync
    fi
    echo -e "\033[32mSuccess, the disk has been formatted!\033[0m"
done
echo -e "\n\033[36mStep 4: The disk is partitioning and formatting\033[0m"
echo -e "\033[33mIs running, please wait!\033[0m"
fdisk_mkfs() {
fdisk -S 56 $1 << EOF
n
p
1


wq
EOF

sleep 2
mkfs.ext4 ${1}1
}
fdisk_mkfs $Disk > /dev/null 2>&1
echo -e "\033[32mSuccess, the disk has been partitioned and formatted!\033[0m"
echo -e "\n\033[36mStep 5: Make a directory and mount it\033[0m"
echo -e -n "\033[33mPlease enter a location to mount (e.g.: /mnt/data):\033[0m"
read Mount
mkdir -p $Mount > /dev/null 2>&1
mount ${Disk}1 $Mount
echo -e "\033[32mSuccess, the mount is completed!\033[0m"
echo -e "\n\033[36mStep 6: Write configuration to /etc/fstab and mount device\033[0m"
UUID=$(blkid -s UUID "${Disk}1" | awk '{ print $2 }' | tr -d '"')
if [ -n "${UUID}" ]; then
    echo "${UUID}" "${Mount}" 'ext4 defaults 0 0' >> /etc/fstab
else
    echo "${Disk}1" "${Mount}" 'ext4 defaults 0 0' >> /etc/fstab
fi
echo -e "\033[32mSuccess, the /etc/fstab is Write!\033[0m"
echo -e "\n\033[36mStep 7: Show information about the file system on which each FILE resides\033[0m"
df -h
sleep 2
echo -e "\n\033[36mStep 8: Show the write configuration to /etc/fstab\033[0m"
cat /etc/fstab
```