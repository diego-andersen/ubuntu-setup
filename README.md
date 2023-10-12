# Ubuntu laptop initial setup
How to get an Ubuntu laptop to blank slate state, ready for work. Does not include pulling dotfiles or additional configuration or installations (e.g. `ranger`, `nnn`, Slack, Postman, etc.).

## System changes
### Remove Snap
1. `$ snap list`
2. Order is important!

	```shell
    $ snap remove firefox
	$ snap remove gtk-common-themes
	$ snap remove gnome-3-38-2004
	$ snap remove snapd-desktop-integration
	$ snap remove snap-store
	$ snap remove core20
	$ snap remove bare
	$ snap remove snapd
    ```

3. `$ sudo apt autoremove --purge snapd`
4. `$ sudo rm -rf /var/cache/snapd`
5. `$ rm -rf ~/snap`
6. `$ sudoedit /etc/apt/preferences.d/nosnap.pref`

    ```shell
    Package: snapd
	Pin: release a=*
	Pin-Priority: -10
	```

### Install Software Center with Flatpak support
https://ubuntuhandbook.org/index.php/2022/04/gnome-software-ubuntu-2204/

1. `$ sudo apt install gnome-software`
2. `$ flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo`
3. `$ sudoedit /usr/share/applications/org.gnome.Software.desktop`

    ```shell
    Name=Gnome Software
    Icon=gnome-software
    ```

### Firefox

1. `$ sudo apt purge firefox`
2. `$ sudoedit /etc/apt/preferences.d/firefox.pref`

    ```shell
    Package: firefox*
	Pin: release o=Ubuntu*
	Pin-Priority: -1
    ```

3. `$ sudo add-apt-repository ppa:mozillateam/ppa`
4. `$ sudo apt update && sudo apt install firefox -y`

### Fix trackpad
https://github.com/bulletmark/libinput-gestures

1. `$ sudoedit /etc/gdm3/custom.conf`

    ```shell
    WaylandEnable=false
    ```

   ... and reboot

2. `$ sudo apt install libinput-tools`
3. `$ git clone https://github.com/bulletmark/libinput-gestures.git`
4. `$ cd libinput-gestures`
5. `$ sudo ./libinput-gestures-setup install`
6. Copy, clone, or modify `~/.config/libinput-gestures.conf`
7. `$ libinput-gestures-setup autostart`

### Desktop panel & extension manager

1. `$ sudo apt install gnome-tweaks gnome-shell-extensions gnome-shell-extension-manager`
2. From Extension Manager (or web) install Dash to Panel and ArcMenu
3. Dash to Panel config > Behavior > Show overview on startup OFF

### Vitals

1. `$ sudo apt install gir1.2-gtop-2.0 lm-sensors`
2. Install Vitals through extension manager/website

### Other extensions
- Gnome Clipboard
- NoAnnoyance v2 (`noannoyance@daase.net`)

### Fix Bluetooth (disable on startup)

`$ sudoedit /etc/bluetooth/main.conf`

    AutoEnable=false

### Remove ESM bullshit
1. `$ sudo apt remove --purge ubuntu-advantage-tools`

## Software
### Neovim

1. AppImage from https://github.com/neovim/neovim/releases
2. `$ sudo chmod u+x ~/Downloads/nvim.appimage`
3. `$ sudo mv ~/Downloads/nvim.appimage /usr/bin/nvim`

### VSCode
https://code.visualstudio.com/docs/setup/linux


1. `# wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > packages.microsoft.gpg`
2. `# sudo install -D -o root -g root -m 644 packages.microsoft.gpg /etc/apt/keyrings/packages.microsoft.gpg`
3. `# sudo sh -c 'echo "deb [arch=amd64,arm64,armhf signed-by=/etc/apt/keyrings/packages.microsoft.gpg] https://packages.microsoft.com/repos/code stable main" > /etc/apt/sources.list.d/vscode.list'`
4. `# rm -f packages.microsoft.gpg`
5. `$ sudo apt install apt-transport-https`
6. `$ sudo apt update && sudo apt install code`

### AWS CLI
https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html

1. `$ curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"`
2. `$ unzip awscliv2.zip`
3. `$ sudo ./aws/install`

### Docker

https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository
