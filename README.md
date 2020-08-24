# Emacs supporting pure-gtk3

In this fork, I'm working to make Emacs support pure-gtk3, in order to support Wayland.

## Building

You need cairo.

```sh
git clone https://github.com/masm11/emacs.git
cd emacs
./autogen.sh
./configure --with-pgtk --with-cairo --with-modules
make
```

Ignore warnings.

#### For archlinux users

This repository contains PKGBUILD, so you can install by:

```
mkdir /tmp/emacs
cd /tmp/emacs
wget https://raw.githubusercontent.com/masm11/emacs/pgtk/PKGBUILD
makepkg -s
sudo pacman -U emacs-pgtk*.tar.xz
```

## Running

```sh
./src/emacs
```

If you want to know whether it is really running on wayland native, then run as follows:

```sh
GDK_BACKEND=wayland ./src/emacs
```

## X11 and Wayland

Of course, PGTK supports X11 and Wayland connections.

You can use `GDK_BACKEND` environment variable and `--display` option,
and you can do `(make-frame-on-display display-name)` with display-name of
different backend from the first frame.

You can know which backend is used for a frame:

```elisp
(pgtk-backend-display-class)
```

This returns `"GdkWaylandDisplay"` for Wayland, or `"GdkX11Display"` for X11.

Note: Segmentation fault may occur on multiple display environment.

## Instead of xrdb

X has the resource database, and you could store initial default values into it.

Gtk/Gdk can't handle it even if on X11, so I implemented similar feature using gsettings.

Saving:
```elisp
(pgtk-set-resource "background" "gray")
```

Getting:
```elisp
(x-get-resource "background" "Background")
```

If your emacs got failing to start, then edit your settings with `dconf-editor`.
Your settings are saved under `/org/gnu/emacs/defaults-by-name/<instance-name>/` and
`/org/gnu/emacs/defaults-by-class/`. All are of string type.
Correct your mistakes.

## TODO

Known problems:
- Segmentation fault while multiple-display.
- Exits when a connection to display server is closed by peer. (I may not be able to resolve.)

Not implemented:
- GTK on wayland does not implement functions for these features:
  - x_set_no_focus_on_map
  - x_set_no_accept_focus
  - x_set_z_group
  - auto-raise/lower
- GTK does not implement functions for these features:
  - vendor_specific_keysyms
- Some other features. Keywords:
  - gtk_plug (not exists on wayland)
  - frame_x_embedded_p

I may not develop them because I don't use them.

## Debugging

Edit src/pgtkterm.h to uncomment:

```c
#define PGTK_DEBUG 1
```

It enables so much debugging outputs.

On gdb, you may want to do:

```
(gdb) handle SIGPIPE nostop noprint
```

## Input Methods

You should be able to use input methods since `GtkIMContext` is enabled
by default.

If you don't like `GtkIMContext`, you can disable it by writing as follows
in `~/.emacs`:

```elisp
(setq pgtk-use-im-context-on-new-connection nil)
```

## My Environment

- archlinux
- gtk+ 3.24.22
- glib2 2.64.4
- gcc 10.1.0
- wayland 1.18.0
- wayland-protocols 1.20
- cairo 1.17.2
- freetype2 2.10.2
- imagemagick 7.0.10.26
- ibus 1.5.22
- mozc 2.23.2815.102
- wayfire 167c3c35

## Notice

We (@masm11 and @fejfighter) are going to push @fejfighter's fork to the fsf's repository, instead of my fork.  @fejfighter's one is rebaseable and have English commit messages.  See #23.

## About me

masm11.
