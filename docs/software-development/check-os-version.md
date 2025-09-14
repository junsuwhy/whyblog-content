---
sidebar_position: 2
title: "Linux, Debian 檢查 OS 版本 Version"
---

## 簡單說明

以下是查看 Debian 系統版本資訊的常用指令和結果範例：

1. 查看 Debian 版本：

   ```
   $ cat /etc/debian_version
   11.6
   ```

2. 查看詳細的發行版資訊：

   ```
   $ lsb_release -a
   No LSB modules are available.
   Distributor ID: Debian
   Description:    Debian GNU/Linux 11 (bullseye)
   Release:        11
   Codename:       bullseye
   ```

3. 查看 Linux 核心版本：

   ```
   $ uname -r
   5.10.0-18-amd64
   ```

4. 系統資訊概覽：

   ```
   $ hostnamectl
      Static hostname: debian-server
            Icon name: computer-vm
              Chassis: vm
           Machine ID: 1234567890abcdef1234567890abcdef
              Boot ID: 0123456789abcdef0123456789abcdef
     Operating System: Debian GNU/Linux 11 (bullseye)
               Kernel: Linux 5.10.0-18-amd64
         Architecture: x86-64
   ```

這些指令可以幫助您快速獲取 Debian 系統的版本資訊。您可以根據需求選擇使用最合適的指令。

## 詳細說明

### 1. lsb_release -a
**指令**:
```bash
lsb_release -a
```
**範例輸出**:
```
No LSB modules are available.
Distributor ID: Debian
Description:    Debian GNU/Linux 10 (buster)
Release:        10
Codename:       buster
```

### 2. cat /etc/debian_version
**指令**:
```bash
cat /etc/debian_version
```
**範例輸出**:
```
10.4
```

### 3. cat /etc/os-release
**指令**:
```bash
cat /etc/os-release
```
**範例輸出**:
```
PRETTY_NAME="Debian GNU/Linux 10 (buster)"
NAME="Debian GNU/Linux"
VERSION_ID="10"
VERSION="10 (buster)"
VERSION_CODENAME=buster
ID=debian
HOME_URL="https://www.debian.org/"
SUPPORT_URL="https://www.debian.org/support"
BUG_REPORT_URL="https://bugs.debian.org/"
```

### 4. uname -a
**指令**:
```bash
uname -a
```
**範例輸出**:
```
Linux hostname 4.19.0-6-amd64 #1 SMP Debian 4.19.67-2+deb10u2 (2019-11-11) x86_64 GNU/Linux
```

### 5. hostnamectl
**指令**:
```bash
hostnamectl
```
**範例輸出**:
```
   Static hostname: debian
         Icon name: computer-laptop
           Chassis: laptop
        Machine ID: 3b2a4aab09a5448883897db00a1d6fa0
           Boot ID: 4e6e4607a5f041e8897d0bb8af9a0b7b
  Operating System: Debian GNU/Linux 10 (buster)
            Kernel: Linux 4.19.0-6-amd64
      Architecture: x86-64
```

這些範例提供了查詢 Debian 系統資訊時的一些基本方式，可以根據實際需要進行選擇和使用。