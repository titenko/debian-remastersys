# Changelog

All notable changes to this project are documented here.
The format is based on [Keep a Changelog](https://keepachangelog.com/),
and this project adheres to Debian-style package versioning.

The detailed per-package Debian changelogs remain at:

* `remastersys/usr/share/doc/remastersys/changelog`
* `remastersys-gui/usr/share/doc/remastersys-gui/changelog`

## [5.0-1] — Debian 13 (trixie) modernization

### Added
* **Hybrid BIOS + UEFI ISO** built with a single `xorriso` call: an
  El Torito BIOS boot image plus a FAT EFI System Partition
  (`efi.img`) built with `mtools`.
* EFI loader is also exposed as plain files in the ISO tree
  (`/EFI/BOOT/BOOTX64.EFI`) so the image UEFI-boots even when copied
  file-by-file onto a FAT USB stick, not only via `dd`.
* `grub_eltorito.img` is generated on the fly with
  `grub-mkimage -O i386-pc-eltorito -p /boot/grub` when a prebuilt one
  is not shipped.
* Repository restructured for GitHub: `docs/`, `assets/`, `.github/`
  templates and CI, plus standard community health files
  (Maksym "Mative" Titenko <titenko.m@gmail.com>).

### Changed
* **live-config** is now written to `/etc/live/config.conf.d/9999-remastersys.conf`
  (modern layout read by live-config 11.x in trixie); the legacy
  `/etc/live/config.conf` is still written for backwards compatibility.
* **squashfs compression switched from zstd to `xz` with the `x86` BCJ
  filter** (`-comp xz -Xbcj x86`) for smaller images.
* `LIVE_NOCONFIGS` refreshed for trixie (dropped removed `gdm`/`kdm`,
  kept `gdm3`, `sddm`, `lightdm`, `xfce4-panel`, …).
* Dependencies aligned with trixie: use `-bin` grub packages
  (`grub-pc-bin`, `grub-efi-amd64-bin`) to avoid the mutually-exclusive
  `grub-pc`/`grub-efi-amd64` metapackage conflict; added `mtools`,
  `dosfstools`, `xz-utils`.
* Installer cleans both `/lib/live` and `/usr/lib/live` on the target.

### Fixed
* `xorriso` failures on 1.5.x: removed unsupported `-file_size_limit off`
  (ISO 9660 level 3 already allows >4 GiB files) and fixed over-quoted
  `-compliance`/`-volid` arguments.
* Safe copy of `/usr/share/grub` (absent on some trixie setups) and
  guaranteed `unicode.pf2` font copy for the GRUB menu.

## [4.9-1] and earlier

See the per-package Debian changelogs listed above. Highlights of the
4.x fork by Daniel Dias Rodrigues:

* Full internationalization (gettext) with Brazilian Portuguese locale.
* Migration to `xorriso` as the standard ISO tool (over `genisoimage`).
* ISO 9660 v2 support (images and single files larger than 4 GiB).
* `ysudo` replacing `gksu`/`gksudo`/`kdesu`.

[5.0-1]: https://github.com/nerun/remastersys/releases
[4.9-1]: https://github.com/nerun/remastersys/releases
