## How I install my system

### Preparing

Copy `.ssh` and `.local/share/gnupg` to `Private`.

Download [Fedora image](https://getfedora.org/ru/workstation/)
and write it to the USB drive:

```sh
sudo ~/Projects/env/prepare-install-drive PATH_TO.iso /dev/sdX
```

Copy these `Vídeos/*` to server and backup machine:

```sh
~/Projects/env/bin/backup
```

### BIOS

1. Boot to BIOS and set supervisor password.
2. Set Game Optimized iGPU.
3. Temporary enable USB boot.

### Install

Start installer:

1. Select English language.
2. Use entire disk mode with encryption.
3. Set host name to `savoia`.

Reboot to BIOS. Block boot from USB.

Reboot to system. Set Russian language, name to `Andrey Sitnik` and login `ai`.

Copy `Projects/` and `Документы/.Private/` from external SDD and open `Install.md` locally.

Enable `Performance`, disable `Automatic waiting saving` in Power settings.

### System Update

Remove unnecessary packages:

```sh
sudo dnf remove cheese rhythmbox gnome-boxesd orca gnome-contacts gnome-getting-started-docs nautilus-sendto gnome-shell-extension-* libreoffice-* gnome-characters gnome-maps gnome-photos simple-scan virtualbox-guest-additions gedit gnome-boxes gnome-tour gnome-connections mediawriter eog gnome-system-monitor baobab gnome-log gnome-calculator gnome-weather gnome-text-editor gnome-font-viewer gnome-clocks gnome-calendar evince totem ffmpeg-free snapshot intel-media-driver cups-browsed anaconda malcontent-control loupe cockpit-bridge cockpit-system cockpit-storaged cockpit-ws cockpit-ws-selinux
```

Run Software Center, enable Flathub and Chrome.

Add RPM Fusion (for codecs):

```sh
sudo dnf install --nogpgcheck http://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm http://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
flatpak remote-delete fedora
```

Update system via Software Center.

Install software:

```sh
sudo dnf install --nogpgcheck --repofrompath 'terra,https://repos.fyralabs.com/terra$releasever' terra-release
sudo dnf swap ffmpeg-free ffmpeg --allowerasing
sudo dnf copr enable dusansimic/themes
sudo dnf copr enable hyperreal/better_fonts
sudo dnf install micro zenity borgbackup openssl ffmpegthumbnailer nss-tools flac gstreamer1-plugin-libav gstreamer1-plugins-bad-freeworld gstreamer1-plugins-ugly mesa-va-drivers-freeworld mpv libva-utils vulkan-tools webp-pixbuf-loader heif-pixbuf-loader avif-pixbuf-loader libheif-tools 7zip speech-dispatcher speech-dispatcher-utils nodejs26-bin podman git tig ripgrep bat make difftastic nextcloud-client zsh util-linux-user starship eza atuin sqlite nethogs fuse-sshfs playerctl cabextract tesseract tesseract-devel podman-compose gdisk tesseract-langpack-rus zbar android-tools solaar wl-clipboard xclip fuse-encfs libgda libgda-sqlite ollama-vulkan zed  morewaita-icon-theme google-chrome-stable cabextract xorg-x11-font-utils fontconfig
sudo rpm -ivh --nosignature --nodigest --nofiledigest https://downloads.sourceforge.net/project/mscorefonts2/rpms/msttcore-fonts-installer-2.6-1.noarch.rpm
```

Set Flatpak languages:

```sh
flatpak config languages --set "es;en;ru"
sudo flatpak update
```

Install applications from Flatpak:

```sh
flatpak install flathub de.haeckerfelix.Fragments org.telegram.desktop org.nickvision.tubeconverter org.gnome.Loupe com.mattjakeman.ExtensionManager io.gitlab.adhami3310.Converter net.nokyan.Resources org.gnome.Calculator org.gnome.Logs org.gnome.Weather org.gnome.clocks org.gnome.Calendar org.gnome.Epiphany org.inkscape.Inkscape org.gnome.gitlab.YaLTeR.VideoTrimmer org.gnome.World.Iotas app.devsuite.Ptyxis hu.irl.cameractrls org.gnome.Snapshot org.gnome.Papers org.gimp.GIMP be.alexandervanhee.gradia com.github.PintaProject.Pinta com.yubico.yubioath org.gnome.font-viewer
```

Remove default GNOME console.

Set `Ctrl + C` and `Ctrl + V` for copy/paste in new terminal settings.

Add Autologin and fingers to user settings.

Disable Software auto-update and notifications.

Install `micro` and its plugins:

```sh
micro -plugin install editorconfig
sudo dnf remove nano
```

Disable waking up by mouse by creating `/etc/udev/rules.d/logitech-bolt.rules`:

```sh
ACTION=="add", SUBSYSTEM=="usb", DRIVERS=="usb", ATTRS{idVendor}=="046d", ATTR{power/wakeup}="disabled"
```

### Personal Files

Copy `.ssh` and `.local/share/gnupg`:

```sh
~/Projects/env/bin/private
```

Change permissions:

```sh
chmod 744 ~/.ssh
chmod 700 ~/.local/share/gnupg/
chmod 644 ~/.ssh/* ~/.local/share/gnupg/*
chmod 700 ~/.local/share/gnupg/private-keys-v1.d
chmod 600 ~/.ssh/id_* ~/.local/share/gnupg/private-keys-v1.d/*
```

Copy configs:

```sh
~/Projects/env/bin/copy-env system
```

## Input

Open Solaar. In the `Key/Button Diversion` row select each button in turn
and set it to `Diverted` instead of `Regular`:

- Back Button
- Forward Button
- Haptic

Install custom universal keyboard layouts:

```sh
mkdir -p ~/.config/xkb/symbols/ ~/.config/xkb/rules/
wget -O ~/.config/xkb/symbols/universal_en https://raw.githubusercontent.com/ai/universal-layout/main/universal_en.xkb
wget -O ~/.config/xkb/symbols/universal_ru https://raw.githubusercontent.com/ai/universal-layout/main/universal_ru.xkb
wget -O ~/.config/xkb/rules/evdev.xml https://raw.githubusercontent.com/ai/universal-layout/main/evdev.xml
```

Set keyboard settings:

```sh
dconf write /org/gnome/desktop/input-sources/xkb-options "['grp_led:caps', 'lv3:ralt_switch', 'grp:shift_caps_switch']"
```

### Terminal

Install zsh:

```sh
mkdir -p ~/.local/share/history
chmod 700 ~/.local/share/history
git clone https://github.com/zsh-users/zsh-syntax-highlighting ~/.local/lib/zsh/zsh-syntax-highlighting
git clone https://github.com/zsh-users/zsh-autosuggestions ~/.local/lib/zsh/zsh-autosuggestions
git clone https://github.com/jimhester/per-directory-history ~/.local/lib/zsh/per-directory-history
chsh -s /bin/zsh
```

Create `/root/.zshrc`:

```sh
eval "$(starship init zsh)"
```

Reboot.

Select `English/Spanish/Catalan Universal` (first) and `Russian Universal` layouts.

```sh
rm ~/.bash*
```

Open backup and copy files from it.

```sh
mkdir ~/backup
borg mount "ai@susedko.local:/var/mnt/vault/ai/backup" ~/backup
```

Copy files.

```sh
borg umount ~/backup
rmdir ~/backup
```

Start Firefox and switch to your language.

Start copying `Vídeos/*` from SDD.

Set [color profile](https://www.notebookcheck.net/uploads/tx_nbc2/BOE0CB4.icm)
in `Color` settings.

### Text Editors

Move Claude Code:

```bash
mkdir -p ~/.config/environment.d/
mkdir -p ~/.local/share/claude
echo "GNUPGHOME=$HOME/.local/share/gnupg" >> ~/.config/environment.d/90-clean-home.conf
echo "NODE_COMPILE_CACHE=$HOME/.cache/node" >> ~/.config/environment.d/90-clean-home.conf
echo "NPM_CONFIG_USERCONFIG=$HOME/.config/npmrc" >> ~/.config/environment.d/90-clean-home.conf
echo "CLAUDE_CONFIG_DIR=$HOME/.local/share/claude" >> ~/.config/environment.d/90-clean-home.conf
```

Sign-in into accounts in Zed.

Login to NextCloud client to `https://sync.sitnik.es`.

Open Iotas app, log-in into Nextcloud account.

### GNOME Settings

Open Clock and add `Vladivostok`, `Moscow`, `Lisbon`, and `San Francisco`.

Run Weather app and set current location.

Install [Lilex](https://lilex.myrt.co).

```sh
mkdir -p ~/.local/share/fonts
# Copy variable fonts to Lilex/
fc-cache -f -v
gsettings set org.gnome.desktop.interface monospace-font-name "Lilex 12"
```

Open settings:

- **Appearance:** use standard GNOME wallpaper.
- **Notifications:** disable Notifications on lock screen.
- **Search:** keep only Calculator and Settings.
- **Multitask:** disable Active corners & Hot corner.
- **Online accounts:** add Google.
- **Power:** enable Show percentage.
- **Mouse:** mouse speed to 75%, touchpad speed to 90%.
- **Date and time:** enable seconds and week day on top panel.
- **Privacy** → disable File History.

Boost volume over 100%:

```sh
gsettings set org.gnome.desktop.sound allow-volume-above-100-percent true
```

Nautilus:

- Enable Sort folders before files.
- Enable Single click to open items.
- Enable preview and search for remote folders.

Disable GNOME extension version check:

```sh
gsettings set org.gnome.shell disable-extension-version-validation true
```

Download the latest [`framework_tool`](https://github.com/FrameworkComputer/framework-system/releases/latest), extract and copy to the system:

```sh
sudo cp ~/Загрузки/framework_tool /usr/local/bin
sudo chmod a+x /usr/local/bin/framework_tool
```

Install tool for clipboard manager:

```bash
mkdir -p ~/.local/share/copyous@boerdereinar.dev/
curl https://cdnjs.cloudflare.com/ajax/libs/highlight.js/11.11.1/es/highlight.min.js > ~/.local/share/copyous@boerdereinar.dev/highlight.min.js
```

Install extensions from [`GNOME.md`](./GNOME.md).

Restore settings file from backup:

```sh
~/Projects/env/bin/restore-gnome-extensions
```

[Disable](https://discussion.fedoraproject.org/t/please-enter-passphrase-for-disk-has-returned/150626/5) disk name in password prompt.

Add ethernet-WiFi switching:

```sh
sudo tee /etc/NetworkManager/dispatcher.d/90-wifi-ethernet >/dev/null <<'EOF'
#!/usr/bin/bash

export LC_ALL=C

ACTION="${2:-}"

case "$ACTION" in
    up|down)
        if /usr/bin/nmcli -t -f TYPE,STATE device status \
            | /usr/bin/grep -qx 'ethernet:connected'; then

            echo "90-wifi-ethernet: Ethernet connected -> Wi-Fi OFF" >&2
            /usr/bin/nmcli radio wifi off
        else
            echo "90-wifi-ethernet: no Ethernet -> Wi-Fi ON" >&2
            /usr/bin/nmcli radio wifi on
        fi
        ;;
esac
EOF

sudo chown root:root /etc/NetworkManager/dispatcher.d/90-wifi-ethernet
sudo chmod 755 /etc/NetworkManager/dispatcher.d/90-wifi-ethernet
sudo restorecon -v /etc/NetworkManager/dispatcher.d/90-wifi-ethernet
```

### Folders

Create empty file template:

```sh
mkdir ~/.local/share/desktop
mkdir ~/.local/share/templates
touch ~/.local/share/templates/Пустой\ файл
```

Fix folders at `~/.config/user-dirs.dirs`:

```sh
XDG_DESKTOP_DIR="$HOME/Загрузки"
XDG_DOWNLOAD_DIR="$HOME/Загрузки"
XDG_TEMPLATES_DIR="$HOME/.local/share/templates"
XDG_PUBLICSHARE_DIR="$HOME/.local/share/desktop"
XDG_DOCUMENTS_DIR="$HOME/Документы"
XDG_MUSIC_DIR="$HOME"
XDG_PICTURES_DIR="$HOME/Загрузки"
XDG_VIDEOS_DIR="$HOME/Видео"
XDG_PROJECTS_DIR="$HOME/Projects"
```

Clean bookmarks:

```sh
echo "" > ~/.config/gtk-3.0/bookmarks
```

Create nested folder:

```sh
mkdir ~/Загрузки/Снимки\ экрана
```

Connect to server in Files by `sftp://ai@susedko.local/` and add `vault` to Favorites places. Add `Загрузки` and `Снимки экрана` to Favorites places.

Left only Telegram, Firefox, Nautilus, Terminal, Iotas, System Update, and
Backup in the dock.

Add icon theme:

```sh
gsettings set org.gnome.desktop.interface icon-theme 'MoreWaita'
```

Set icon `/usr/share/icons/Adwaita/scalable/places/folder-pictures.svg`
for `~/Загрузки/Снимки экрана/`.

## Home Server

Add server’s certificate to the system:

```sh
mkdir -p ~/.pki/nssdb
certutil -N -d sql:$HOME/.pki/nssdb --empty-password
certutil -d sql:$HOME/.pki/nssdb -A -t "C,," -n sitnik -i ~/Projects/susedko/sitniks.crt
sudo cp ~/Projects/susedko/sitniks.crt /etc/pki/ca-trust/source/anchors/sitniks.pem
sudo update-ca-trust
```

Add service to `~/.config/systemd/user/force-lock.service`:

```ini
[Unit]
Description=Force Lock

[Service]
ExecStart=/home/ai/Projects/env/bin/force-lock
Restart=on-failure
RestartSec=30s

[Install]
WantedBy=default.target
```

Add service to `~/.config/systemd/user/domovoy-listener.service`:

```ini
[Unit]
Description=Domovoy Listener
After=network-online.target
Wants=network-online.target

[Service]
ExecStart=/home/ai/Projects/env/bin/domovoy-listener
Restart=on-failure
RestartSec=30s
StartLimitBurst=5

[Install]
WantedBy=default.target
```

Enable services:

```sh
systemctl --user enable domovoy-listener.service force-lock.service
systemctl --user start domovoy-listener.service force-lock.service
```

### Development Tools

Install [Zoom](https://www.zoom.com/download).

Install Node.js, TypeScript, and Dev Containers.

```sh
npm config set cache "$HOME/.cache/npm"
mkdir -p ~/.local/lib/node/
tee -a ~/.local/lib/node/package.json << EOM
{
  "dependencies": {
    "@devcontainers/cli": ">=0.80.1"
  }
}
EOM
cd ~/.local/lib/node && npm install && cd

mkdir -p ~/.local/share/zed_server
mkdir -p ~/.local/share/multiocular
mkdir -p ~/.local/share/history
chmod 700 ~/.local/share/history
```

Fix Dev Container with Podman in Zed:

```sh
sudo ln -s $(which podman) /usr/local/bin/docker
```

Create `~/.config/containers/containers.conf`:

```ini
[containers]
userns = "keep-id"
label = false
```

Install Keybase:

```sh
sudo dnf install https://prerelease.keybase.io/keybase_amd64.rpm
run_keybase
```

Disable autostart in Keybase settings and revoke old laptop.

## LanguageTool Server

Prepare [ngrams](https://languagetool.org/download/ngram-data/):

```sh
mkdir -p ~/.local/share/ngrams
cd ~/.local/share/ngrams
wget https://languagetool.org/download/ngram-data/ngrams-en-20150817.zip
wget https://languagetool.org/download/ngram-data/ngrams-es-20150915.zip
wget https://languagetool.org/download/ngram-data/untested/ngram-ru-20150914.zip
unzip ngrams-en-20150817.zip
unzip ngrams-es-20150915.zip
unzip ngram-ru-20150914.zip
rm ngram*.zip
```

Create service unit `~/.config/systemd/user/languagetool.service`:

```ini
[Unit]
Description=LanguageTool Server

[Service]
ExecStart=podman run --rm --replace --name languagetool \
  -p 8081:8010 \
  -e langtool_languageModel=/ngrams \
  -e Java_Xms=512m -e Java_Xmx=2g \
  -v /home/ai/.local/share/ngrams:/ngrams:Z \
  docker.io/erikvl87/languagetool:latest

[Install]
WantedBy=default.target
```

Enable service.

```sh
systemctl --user daemon-reload
systemctl --user enable --now languagetool.service
systemctl --user start --now languagetool.service
```
