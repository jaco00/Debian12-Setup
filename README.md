# Debian 软件安装指南 (Debian Software Installation Guide)

## 1. 安装 NVIDIA 驱动 (Install NVIDIA Driver)

📌 **适用环境** (Applicable Environment)：  
本指南适用于 Debian 12 "Bookworm" amd64 版本
其他版本或架构请参考官方文档：[Debian Wiki](https://wiki.debian.org/NvidiaGraphicsDrivers) 

**Prerequisites**
```bash
sudo apt install linux-headers-amd64
```

 **更新 APT 源 (Update APT Sources)**

编辑 `/etc/apt/sources.list` 添加以下内容 (Modify `/etc/apt/sources.list` and add the following lines):
```bash
deb http://deb.debian.org/debian/ bookworm main contrib non-free non-free-firmware
```

**更新 APT 缓存 (Update APT Cache)**
```bash
sudo apt update
```

**安装 NVIDIA 驱动 (Install NVIDIA Driver)**
```bash
sudo apt install nvidia-driver firmware-misc-nonfree
```

**强制启用 Wayland 下的 NVIDIA DRM (Force NVIDIA DRM for Wayland and Restart)**
```bash
sudo echo "options nvidia-drm modeset=1" >> /etc/modprobe.d/nvidia-options.conf
sudo reboot
```

---

## 2. 设置基本权限 (Set Basic Permissions)

将用户 `$username$` 添加到 `sudo` 组 (Add user `$username$` to the `sudo` group):
```bash
sudo usermod -aG sudo $username$
```

修正 `_apt` 用户权限 (Fix `_apt` user permission issue):
```bash
sudo setfacl -m u:_apt:x /home/$username$/
```

---

## 3. 安装中文字体 (Install Chinese Fonts)

```bash
sudo apt install fonts-droid-fallback ttf-wqy-zenhei ttf-wqy-microhei fonts-arphic-uming
```

手动安装 Windows 字体 (Manually Install Windows Fonts):
```bash
sudo mkdir /usr/share/fonts/windows
sudo cp ~/fonts/* /usr/share/fonts/windows
```

更新字体缓存 (Update Font Cache):
```bash
sudo fc-cache -fv
```

重新配置语言环境 (Reconfigure Locales):
```bash
sudo dpkg-reconfigure locales
```

---

## 4. 安装 VIM (Install VIM)

```bash
sudo apt install net-tools vim-nox vim-gtk3
```

将 `vim.nox` 设为默认 VIM 版本 (Set `vim.nox` as the default VIM version):
```bash
sudo rm /usr/bin/vim
sudo ln -s /usr/bin/vim.nox /usr/bin/vim
```

禁用鼠标支持 (Disable Mouse Support in VIM):
```bash
sudo sed -i 's/\(.*\)set mouse=a/\1set mouse-=a/' /usr/share/vim/vim90/defaults.vim
```

---


## 5.Tabby 终端 & Tmux / Tabby Terminal & Tmux

```sh
wget https://github.com/Eugeny/tabby/releases/download/v1.0.221/tabby-1.0.221-linux-x64.deb                                                                      sudo apt install ./tabby-1.0.221-linux-x64.deb                                  sudo apt install tmux
```
---

## 6.Rime 输入法 / Rime Input Method

```sh
sudo apt install fcitx5 fcitx5-configtool fcitx5-frontend-gtk3 fcitx5-frontend-qt5 fcitx5-chinese-addons
im-config -n fcitx5
sudo apt-get install fcitx5-rime
language add chinese
fcitx configure add rime   # 重新登录（如有需要）
```
---
## 7.Rime-Ice 配置 / Rime-Ice Configuration

```sh
git clone https://github.com/iDvel/rime-ice.git Rime --depth 1
mv ~/.local/share/fcitx5/rime ~/.local/share/fcitx5/rime.old
cp Rime ~/.local/share/fcitx5/rime -R
cd ~/.local/share/fcitx5/rime
vim default.yaml   # 删除无用的方案
```
---
## 8.办公软件 / Office Software

```sh
sudo apt install libreoffice
```
---
## 9.Joplin 笔记 / Joplin Notes

```sh
wget https://github.com/laurent22/joplin/releases/download/v3.2.12/Joplin-3.2.12.deb
sudo apt install ./Joplin-3.2.12.deb
```
---
## 10.OneDrive 云同步 / OneDrive Cloud Sync

```sh
wget -qO - https://download.opensuse.org/repositories/home:/npreining:/debian-ubuntu-onedrive/Debian_12/Release.key | gpg --dearmor | sudo tee /usr/share/keyrings/obs-onedrive.gpg > /dev/null
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/obs-onedrive.gpg] https://download.opensuse.org/repositories/home:/npreining:/debian-ubuntu-onedrive/Debian_12/ ./" | sudo tee /etc/apt/sources.list.d/onedrive.list
sudo apt-get update; sudo apt install --no-install-recommends --no-install-suggests onedrive
onedrive   # 进行身份认证
onedrive --sync --verbose
systemctl --user enable onedrive
systemctl --user start onedrive
```
---

## 11.Google Chrome 浏览器 / Google Chrome Browser

```sh
wget -qO- https://dl.google.com/linux/linux_signing_key.pub | gpg --dearmor | sudo tee /usr/share/keyrings/google-chrome.gpg > /dev/null
echo "deb [signed-by=/usr/share/keyrings/google-chrome.gpg] http://dl.google.com/linux/chrome/deb/ stable main" | sudo tee /etc/apt/sources.list.d/google-chrome.list
sudo apt update && sudo apt install -y google-chrome-stable
```

## 12.VS Code 编辑器 / VS Code Editor

```sh
wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor | sudo tee /usr/share/keyrings/microsoft.gpg > /dev/null
echo "deb [signed-by=/usr/share/keyrings/microsoft.gpg] https://packages.microsoft.com/repos/code stable main" | sudo tee /etc/apt/sources.list.d/vscode.list
sudo apt update && sudo apt install -y code
```

## 13.KeePassXC 密码管理器 / KeePassXC Password Manager

```sh
sudo apt install keepassxc
```
---
## 14.硬件信息工具 / Hardware Information Tools
### lm-sensors 硬件传感器 / lm-sensors Hardware Sensors

```sh
sudo apt install lm-sensors
sudo sensors-detect
sudo sensors
```

### hardinfo cpu-x

```sh
sudo apt install hardinfo cpu-x
```
---
## 15.Motrix 下载器 / Motrix Downloader

```sh
sudo snap install snapd
sudo snap install motrix  # 可能需要重新登录
```
---
## 16.Meld 文件对比工具 / Meld File Comparison Tool

```sh
sudo apt install meld
```
---


## 17.媒体播放器 / Media Players

```sh
sudo apt install vlc
sudo snap install mpv
```
---

## 18.虚拟机 / Virtual Machine

```sh
wget https://download.virtualbox.org/virtualbox/7.1.6/virtualbox-7.1_7.1.6-167084~Debian~bookworm_amd64.deb
sudo apt install ./virtualbox-7.1_7.1.6-167084~Debian~bookworm_amd64.deb
```
---




## 🎯 总结 (Summary)

本指南介绍了在 Debian 上安装 NVIDIA 驱动、配置基本权限、安装中文字体、常用开发工具、以及常用软件。
