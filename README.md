# ubuntu-or-linux-mint-setup

Scripts and instructions for setting up Ubuntu or Linux Mint with tools for development, productivity, graphics, remote control, gaming, multimedia, communication, and more.

## Scripts

* [`install-tools.sh`](install-tools.sh): Scripts for setting up Ubuntu or Linux Mint with recommended drivers and tools for C/C++, Python3, Java8, Java11, Java17, Java21, Node.js, Rust, Go, Ruby, Perl, .NET, GitHub CLI, GitLab CLI, OpenSSL, OpenSSH, JQ, Ghostscript, FFMPEG, Maven, Zsh, Fcitx5, Flatpak, TeX Live, Pandoc, CopyQ, Tailscale, Noto CJK fonts, XITS fonts, Node.js packages, Python3 packages, pipx, Poetry, RARLAB UnRAR, Fabric, Visual Studio Code, Code::Blocks, PowerShell, ANTLR 4, Steam, Discord, Telegram, Spotify, VLC, OBS Studio, LibreOffice, OnlyOffice, Joplin, Calibre, Postman, GIMP, Krita, HandBrake, MuseScore, Aisleriot Solitaire, custom `~/.profile`, custom `~/.bashrc`, custom `~/.vimrc`, and more.
* [`virtualgl-turbovnc.sh`](virtualgl-turbovnc.sh): Scripts for setting up VirtualGL and TurboVNC on Ubuntu or Linux Mint, compatible with NVIDIA GPU.
* [`waydroid.sh`](waydroid.sh): Scripts for setting up Waydroid on Ubuntu or Linux Mint.
* [`wine.sh`](wine.sh): Scripts for setting up Wine on Ubuntu or Linux Mint.

## Instructions

### GRUB

#### When Dual Booting with Windows

When dual booting with Windows, you need to:
- Disabe fast boot (in some context also secure boot) in BIOS.
- In some context, `sudo nano /etc/grub.d/30_os_prober` and add or edit line `quick_boot="0"`.
- `sudo nano /etc/default/grub` and add or edit line `GRUB_DISABLE_OS_PROBER=false`.
- `sudo nano /etc/default/grub` and add or edit non-zero `GRUB_TIMEOUT`, when `GRUB_TIMEOUT_STYLE=menu`, or `GRUB_HIDDEN_TIMEOUT`, when otherwise.

#### GRUB Menu

- When `GRUB_TIMEOUT_STYLE=menu`, after `GRUB_TIMEOUT`, highlighted option will be booted.
- When `GRUB_TIMEOUT_STYLE=hidden` or `GRUB_TIMEOUT_STYLE=countdown`, during `GRUB_HIDDEN_TIMEOUT`, press `ESC` to enter GRUB menu.
- In menu, default highlighted option is the default option. 

#### GRUB Configuration

```
sudo nano /etc/default/grub
``` 

to edit configuration, 

```
sudo update-grub
sudo reboot
```

to apply.

Variables:

- `GRUB_DEFAULT=<number>`: Default boot option to boot. Options and their numbers are showed in GRUB menu.
- `GRUB_TIMEOUT_STYLE=<string>`: GRUB timeout style when booting.
  - `menu`: Show menu, wait until `GRUB_TIMEOUT` ends, and boot default option.
  - `hidden`: Hide menu with black screen, wait until `GRUB_HIDDEN_TIMEOUT` ends, and boot highlighted option. Show menu when `ESC` is pressed during `GRUB_HIDDEN_TIMEOUT`.
  - `countdown`: Hide menu with countdown shown on screen, wait until `GRUB_HIDDEN_TIMEOUT` ends, and boot default option. Show menu when `ESC` is pressed during `GRUB_HIDDEN_TIMEOUT`.
- `GRUB_TIMEOUT=<number>`: When `GRUB_TIMEOUT_STYLE=menu`, the timeout before booting into highlighted option, `-1` for forever. Some versions may need `0.0` for `0`.
- `GRUB_HIDDEN_TIMEOUT=<number>`: When `GRUB_TIMEOUT_STYLE=hidden` or `GRUB_TIMEOUT_STYLE=countdown`, the timeout before booting into the default option. Some versions may need `0.0` for `0`.
- `GRUB_HIDDEN_TIMEOUT_QUIET=<boolean>`: DEPRECATED. `GRUB_TIMEOUT_STYLE=hidden` and `GRUB_HIDDEN_TIMEOUT_QUIET=false` is equivalent to `GRUB_TIMEOUT_STYLE=countdown`; `GRUB_TIMEOUT_STYLE=hidden` and `GRUB_HIDDEN_TIMEOUT_QUIET=true` is equivalent to `GRUB_TIMEOUT_STYLE=hidden`. There's a known bug that make this not work as expected in some versions after this is deprecated.
- `GRUB_DISABLE_OS_PROBER=<boolean>`: Whether to disable OS prober. Set it to `false` when dual booting.

### Driver Manager in Linux Mint

You can install drivers (including NVIDIA driver) with `Driver Manager`, a GUI tool, on Linux Mint.

### NVIDIA

<ul>
<li>You can check NVIDIA driver with <code>nvidia-smi</code>.</li>
<li>You can install CUDA Toolkit with:
<pre><code>sudo apt install nvidia-cuda-toolkit -y
</code></pre>
and check it with <code>nvcc --version</code>.
</ul>

### Steam

#### Install Steam

<ol>
<li>If you have run <a href="install-tools.sh"><code>install-tools.sh</code></a>, go to next step; otherwise, run:
<pre><code>cd ~
sudo dpkg --add-architecture i386
sudo apt update
sudo apt install libgl1:i386 -y
wget https://cdn.fastly.steamstatic.com/client/installer/steam.deb
sudo dpkg -i steam*.deb
rm steam*.deb
</code></pre></li>
<li>Run <code>steam</code> to update and open it for the first time.</li>
<li>Follow the instructions.</li>
<li>Restart Steam.</li>
<li>Follow the instructions.</li>
</ol>

#### Proton Engine

1. Open Steam.
2. Click `Steam` on the menu bar (upper left corner) and click `Settings`.
3. Click `compatibility`.
4. Toggle on `Enable Steam Play for all other titles` if such option exists.
5. Select the Proton engine you want.
6. Restart Steam. 

### Fcitx5

You can configure Fcitx5 in `Fcitx Configuration`, a GUI tool.

### Time Mismatches When Dual Booting with Windows

If time mismatches real local time when dual booting with Windows, do the following steps:

<ol>
<li>In Linux, run:
<pre><code>sudo timedatectl set-local-rtc 0
sudo timedatectl set-ntp true
</code></pre></li>
<li>Boot into Windows and sync time in it if time still mismatches after step 1.</li>
</ol>

### Linux Mint Ubuntu Version Tweak

To make a script for Ubuntu work for both Ubuntu and Linux Mint, do the following tweaks:

1. `$(lsb_release -cs)`: Add `source /etc/os-release` before it and replace it with `$UBUNTU_CODENAME`.
2. `$VERSION_ID` (from `source /etc/os-release`): Add
```
export UBUNTU_VERSION_ID=$(
if grep -q '^NAME="Linux Mint"' /etc/os-release; then
    inxi -Sx | awk -F': ' '/base/{print $2}' | awk '{print $2}'
else
    . /etc/os-release
    echo "$VERSION_ID"
fi
)
```
before it and replace it with `$UBUNTU_VERSION_ID`. This has been added to `~/.bashrc` in [`install-tools.sh`](install-tools.sh).

### Android as SSH and VNC/X Client

See my [**Android-Non-Root**](https://github.com/Willie169/Android-Non-Root).
