# Install niri Wayland window manager using Nix package manager on any Linux distribution

## General information

**niri** – a scrollable-tiling Wayland compositor<br>
https://github.com/YaLTeR/niri/

<br>

**Nix wiki**<br>
https://wiki.nixos.org/wiki/Nix_package_manager
<br>
https://wiki.nixos.org/wiki/Nixpkgs

<br>

**Nixpkgs search** – the Nix Packages collection<br>
https://search.nixos.org/packages

<br>

## Set up nixpkgs environment

```
$ yourpkgmgr install nix    # or nix-bin, or similar
```

```
$ gpasswd -a yourusername nix-users
$ exit
$ systemctl enable --now nix-daemon.service
```

```
$ nix-channel --add https://nixos.org/channels/nixpkgs-unstable
$ nix-channel --update
$ nix-env --install --attr nix-bash-completions
```

`nixpkgs-unstable` channel corresponds to the main development branch (unstable) of Nixpkgs, delivering the latest tested updates on a rolling basis.

<br>

`~/.profile`
```
#export NIX_PATH="$HOME/.nix-defexpr/channels"
PATH="$HOME/.nix-profile/bin:$PATH"
XDG_DATA_DIRS="$HOME/.nix-profile/share:$XDG_DATA_DIRS"
```

<br>

`~/.config/systemd/user.conf`

```
[Manager]
ManagerEnvironment="XDG_DATA_DIRS=%h/.nix-profile/share:/usr/local/share:/usr/share"
```

```
$ systemctl --user daemon-reload
```

<br>

## Install niri and other components

```
$ nix-env -iA niri
```

```
$ nix-env -iA mesa
$ nix-env -iA xwayland-satellite
$ nix-env -iA waybar
$ nix-env -iA alacritty
$ nix-env -iA fuzzel
```

**Complete list of recommended packages to install**

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

## Make fonts from nixpkgs available

```
$ mkdir -p ~/.local/share/fonts
$ ln -s ~/.nix-profile/share/fonts ~/.local/share/fonts/nix-fonts
```

<br>

## Launch niri

#### from Console

```
$ nixGL niri-session
```

#### from Login Manager

```
$ cp ~/.nix-profile/share/wayland-sessions/niri.desktop /usr/share/wayland-sessions/niri-nix.desktop
```

<br>

`/usr/share/wayland-sessions/niri-nix.desktop`

```
Exec=nixGL niri-session
```

<br>

## Customize configuration of individual tools

```
$ cp -r ~/.nix-profile/etc/xdg/waybar ~/.config/
$ cp -r ~/.nix-profile/etc/xdg/fuzzel ~/.config/
```

<br>

## Keep all packages up-to-date

```
$ nix-channel --update
$ nix-env --upgrade
```

<br>

## Try to fix failure to launch or black screen after Mesa updates

```
$ nix-env -iA nixpkgs.mesa
```

or

```
$ nix-env -iA nixgl.auto.nixGLDefault
```

If all else fails, roll back generations.

```
$ nix-env --list-generations
$ nix-env --rollback
```

Repeat until working state has been restored.

<br>

## Clean up

Perform only when everything works properly. After this, rollback is not possible.

```
$ nix-collect-garbage --delete-old
```
