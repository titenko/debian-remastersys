# Security Policy

## Supported versions

| Version | Supported          | Target            |
| ------- | ------------------ | ----------------- |
| 5.x     | :white_check_mark: | Debian 13 trixie  |
| 4.x     | :x:                | Debian 11 legacy  |

## Reporting a vulnerability

Remastersys runs as **root** and manipulates bootloaders, partitions
and system images, so security reports are taken seriously.

Please **do not** open a public issue for a security vulnerability.
Instead, report it privately:

* Use GitHub's **"Report a vulnerability"** (Security Advisories) on the
  repository, or
* email the maintainer: Daniel Dias Rodrigues
  <danieldiasr@gmail.com>.

Include:

* A description of the issue and its impact (e.g. privilege escalation,
  arbitrary file write as root, insecure temp file handling).
* Steps to reproduce.
* The affected script/file and, if known, a suggested fix.

You can expect an acknowledgement within a reasonable time frame. Once a
fix is available, a new release and advisory will be published.
