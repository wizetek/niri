# Install niri using Nix package manager on any Linux distribution

**niri** – a scrollable-tiling Wayland compositor<br>
https://github.com/YaLTeR/niri/

<br>

**nixpkgs** – the Nix Packages collection<br>
https://search.nixos.org/packages

<br>

## Set up nixpkgs environment

```
$ yourpkgmgr install nix
$ gpasswd -a yourusername nix-users
$ exit
$ systemctl enable --now nix-daemon.service
```

```
$ nix-channel --add https://nixos.org/channels/nixpkgs-unstable
$ nix-channel --update
```

`~/.profile`
```
#export NIX_PATH="$HOME/.nix-defexpr/channels"
PATH="$HOME/.nix-profile/bin:$PATH"
XDG_DATA_DIRS="$HOME/.nix-profile/share:$XDG_DATA_DIRS"
```

`~/.config/systemd/user.conf`
```
[Manager]
ManagerEnvironment="XDG_DATA_DIRS=/home/yourusername/.nix-profile/share:/usr/local/share:/usr/share"
```
`$ systemctl --user daemon-reload`

<br>

## Install niri and other components from nixpkgs

```
$ nix-env --install -A niri
$ nix-env -iA waybar
$ nix-env -iA alacritty
$ nix-env -iA xwayland-satellite
```

<br>

## Recommended nixpkgs packages to install

```
$ nix-env --query
alacritty-0.15.1
brightnessctl-0.5.1
font-awesome-6.7.2
fuzzel-1.11.1
gammastep-2.0.9
mako-1.10.0
mesa-25.0.3
niri-25.02
nix-bash-completions-0.6.8
swaybg-1.2.1
swayidle-1.8.0
waybar-0.12.0
waypaper-2.5
xwayland-satellite-0.5.1
```

<br>

## Install NixGL wrapper

```
$ nix-channel --add https://github.com/nix-community/nixGL/archive/main.tar.gz nixgl
$ nix-channel --update nixgl
$ nix-env -iA nixgl.auto.nixGLDefault
```

<br>

## Make fonts from nixpkgs work

```
$ mkdir -p ~/.local/share/fonts
$ ln -s ~/.nix-profile/share/fonts ~/.local/share/fonts/nix-fonts
```

## Launch niri

`$ nixGL niri-session`
