# REMASTERSYS

<a href="LICENSE"><img alt="License: GPLv3" src="https://img.shields.io/badge/License-GPLv3-blue" /></a>
<a href="https://www.debian.org/"><img alt="System: Debian 13 trixie" src="https://img.shields.io/badge/System-Debian%2013%20trixie-blue" /></a>
<a href="https://github.com/nerun/remastersys/releases"><img alt="Version: 5.0" src="https://img.shields.io/badge/Version-5.0%20%28trixie%29-brightgreen" /></a>
<a href="https://github.com/nerun/remastersys/actions"><img alt="CI" src="https://github.com/nerun/remastersys/actions/workflows/ci.yml/badge.svg" /></a>
<a href="https://github.com/nerun/remastersys/graphs/contributors"><img alt="Maintained by: Maksym Mative Titenko" src="https://img.shields.io/badge/Maintained%20by-Maksym%20%22Mative%22%20Titenko-blueviolet" /></a>

**Remastersys** turns an existing **Debian** installation into a bootable
Live ISO — either a full backup you can reinstall anywhere, or a clean
distributable image to share.

> This is a fork of the famous
> [Remastersys (ver. 3.0.0-1)](https://web.archive.org/web/20130423105647/http://www.remastersys.com/),
> developed by Tony "Fragadelic" Brijeski and discontinued in April 2013.

## About this fork

This repository is a **fork of the
[nerun/remastersys](https://github.com/nerun/remastersys) fork** (the
4.x series maintained by Daniel Dias Rodrigues "Nerun"). It is maintained
by **Maksym "Mative" Titenko &lt;titenko.m@gmail.com&gt;**.

### What changed relative to the last nerun fork (4.9-1 → 5.0-1)

* **Debian 13 (trixie) as the target** — the 4.x fork targeted Debian 11;
  this fork is modernized and tested on trixie.
* **Hybrid BIOS + UEFI ISO** — built with a single `xorriso` call (was a
  fragile `grub-mkrescue`/isolinux split). The EFI loader is also exposed
  as `/EFI/BOOT/BOOTX64.EFI` in the ISO tree so it boots from a USB stick
  written file-by-file, not only via `dd`.
* **live-config** is now written to `/etc/live/config.conf.d/9999-remastersys.conf`
  (the layout read by live-config 11.x on trixie), instead of the legacy
  `/etc/live/config.conf`.
* **squashfs compression switched from zstd to `xz`** with the `x86` BCJ
  filter (smaller images).
* **Refreshed dependencies** for trixie: `-bin` grub packages
  (`grub-pc-bin`, `grub-efi-amd64-bin`) to avoid the mutually exclusive
  `grub-pc`/`grub-efi-amd64` metapackage conflict, plus `mtools`,
  `dosfstools`, `xz-utils`.
* **Cleaned up legacy code** — removed dead bootloader/DM references
  (`gdm`, `kdm`, `grub-mkrescue` branches, `xresprobe`).
* **Repository restructured for GitHub publication** — `docs/`, `assets/`,
  `.github/` issue/PR templates and CI, plus standard community health
  files (AUTHORS, CONTRIBUTING, SECURITY, CODE_OF_CONDUCT, CHANGELOG).

See [CHANGELOG.md](CHANGELOG.md) for the full per-version list.

## Project status

* **Version:** 5.0-1 — **tested working** on Debian 13 (trixie): a real
  `remastersys dist` run produced a bootable hybrid BIOS+UEFI ISO
  (verified to contain both `/boot/grub/grub_eltorito.img` and
  `/efi.img`).
* **Compression:** `xz` (`-Xbcj x86`). On the test machine xz produced a
  ~7.7% smaller squashfs than zstd, at the cost of much longer build time
  (minutes vs. over an hour on low-end hardware).
* **Packages:** `remastersys` (CLI) + `remastersys-gui` (YAD) build via
  `./package-creator` → `dpkg-deb`.
* **Issue tracker / PRs:** GitHub Issues and Pull Requests (templates in
  `.github/`). CI runs `bash -n` + `shellcheck` on every push/PR.

## Table of contents

* [What it does](#what-it-does)
* [About this fork](#about-this-fork)
* [Project status](#project-status)
* [Supported systems](#supported-systems)
* [Installation](#installation)
* [Usage](#usage)
* [Repository layout](#repository-layout)
* [Changelog](#changelog)
* [Contributing](#contributing)
* [License](#license)

## What it does

* **Backup** — a full system backup, *including* personal data, to a Live
  ISO you can boot anywhere and install.
* **Dist** — a distributable copy to share; this does *not* contain any of
  your personal user data.

Burn the resulting image onto a USB stick (with *Remastersys Bootable USB*
or a tool of your choice like
[Balena Etcher](https://github.com/balena-io/etcher) or
[Ventoy](https://github.com/ventoy/Ventoy)), insert it and reboot. The
5.0 image is a **hybrid BIOS + UEFI** ISO, so it boots on both legacy and
modern hardware.

## Supported systems

Remastersys targets **Debian**. Please note:

* MX Linux is not Debian.
* Mint is not Debian.
* Ubuntu is not Debian.
* Debian 12 is not Debian 11.

**Debian 13 (trixie) is the target of the 5.0 series.** This release:

* writes live-config to `/etc/live/config.conf.d/`;
* builds a hybrid BIOS+UEFI ISO via `xorriso` (with a real EFI System
  Partition and `/EFI/BOOT/BOOTX64.EFI` in the tree);
* compresses the squashfs with `xz` (`-Xbcj x86`);
* uses trixie-current package names (`live-boot`, `live-config`,
  `live-config-systemd`, `syslinux`, `grub-efi-amd64-bin`, `mtools`,
  `dosfstools`).

## Installation

Prebuilt `.deb` packages, when available, are on the
[releases](https://github.com/nerun/remastersys/releases) page.

To build them yourself:

```shell
./package-creator          # bumps version, regenerates metadata, builds .deb
sudo apt install ./remastersys*.deb
```

This produces two packages:

* **remastersys** — the program itself (terminal mode);
* **remastersys-gui** — the graphical (YAD) interface.

## Usage

```shell
man remastersys
sudo remastersys --help

sudo remastersys backup    # full Live ISO including your data
sudo remastersys dist      # distributable Live ISO without user data
sudo remastersys clean     # remove the work directory
```

With the GUI package installed, these entries appear in your menu:

* Remastersys Bootable USB
* Remastersys Creator
* Remastersys GRUB Restorer
* Remastersys Installer

## Repository layout

```
.
├── remastersys/            # CLI package (fakeroot tree: DEBIAN/ + etc/ + usr/)
├── remastersys-gui/        # GUI package (YAD frontend)
├── package-creator         # builds both .deb packages
├── docs/                   # developer notes, translation tutorial
├── assets/                 # source artwork (.xcf)
├── .github/                # issue/PR templates, CI workflow
├── CHANGELOG.md            # human-readable changelog
├── CONTRIBUTING.md         # how to build, test and contribute
├── SECURITY.md             # how to report vulnerabilities
├── CODE_OF_CONDUCT.md
├── AUTHORS.md
└── LICENSE                 # GPLv3
```

The full per-package Debian changelogs live at
`remastersys/usr/share/doc/remastersys/changelog` and
`remastersys-gui/usr/share/doc/remastersys-gui/changelog` (read them with
`zless` after install, e.g.
`zless /usr/share/doc/remastersys/changelog.gz`).

## Changelog

See [CHANGELOG.md](CHANGELOG.md). Highlights of the fork over the original
Remastersys:

* Full internationalization (gettext; English + Brazilian Portuguese).
* `xorriso` as the standard ISO tool (over the outdated `genisoimage`).
* ISO 9660 v2 support — images and single files larger than 4 GiB.
* `xz` squashfs compression (was gzip, then zstd).
* Hybrid BIOS + UEFI boot support (5.0, Debian 13 trixie).
* `ysudo` replacing `gksu`/`gksudo`/`kdesu`.

## Contributing

Contributions are welcome — see [CONTRIBUTING.md](CONTRIBUTING.md) and the
[developer notes](docs/DEVELOPER_NOTES.md). Translators, see
[docs/TRANSLATING.md](docs/TRANSLATING.md).

## License

Remastersys is released under the **GNU General Public License v3.0**.
See [LICENSE](LICENSE).
