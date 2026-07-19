# Contributing to Remastersys

Thanks for your interest in improving Remastersys! This project is a
community-maintained fork of the original Remastersys, modernized for
**Debian 13 (trixie)**.

## Scope

Remastersys targets **Debian**. Please keep in mind:

* MX Linux, Mint, Ubuntu and Devuan are *not* Debian.
* The 5.0 series targets **Debian 13 (trixie)**.

Patches that add support for derivatives are welcome, but must not break
the Debian target.

## Development setup

The two Debian packages live as fakeroot trees in the repo:

```
remastersys/         # CLI package (the program itself)
remastersys-gui/     # YAD-based graphical frontend
```

Each contains a `DEBIAN/` control tree plus `etc/` and `usr/` payloads.

### Building the .deb packages

```shell
./package-creator
```

It bumps the version, regenerates changelogs, `md5sums` and conffile
hashes, gzip-compresses man pages/changelogs, and runs `dpkg-deb -b`.

### Installing what you built

```shell
sudo apt install ./remastersys*.deb
```

## Coding conventions

* All executables are **POSIX/bash shell scripts**. Keep them
  `bash -n` clean and, ideally, `shellcheck`-clean.
* Match the surrounding style (indentation, quoting, message helpers
  like `log_msg` and the color variables from `libremastersys.sh`).
* User-facing strings are internationalized with `gettext` (`$"..."`).
  If you add a string, keep it translatable and update the `.pot`/`.po`
  where practical (see [docs/TRANSLATING.md](docs/TRANSLATING.md)).
* Prefer modern tooling already required by the package: `xorriso`,
  `mksquashfs`, `grub-mkimage`, `mtools`.

## Submitting changes

1. Fork the repository and create a topic branch.
2. Make your change; verify scripts pass `bash -n` (and `shellcheck`
   if available).
3. If behavior changed, test a real build: `sudo remastersys dist`.
4. Open a Pull Request describing **what** changed and **why**, and on
   which Debian release you tested.

## Reporting bugs

Open an issue using the bug template. Please include:

* Debian version (`cat /etc/debian_version`) and architecture.
* The exact command you ran (`remastersys backup` / `dist` / `iso`).
* Relevant lines from `/home/remastersys/remastersys.log`.

By contributing you agree that your work is licensed under the
project's **GPLv3** license.
