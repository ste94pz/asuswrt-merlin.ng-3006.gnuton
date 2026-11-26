# Asuswrt-Merlin.ng — 3006 / WiFi 7

[![Build](https://github.com/Asuswrt-Merlin/asuswrt-merlin.ng-3006.gnuton/actions/workflows/github-actions.yml/badge.svg?branch=master)](https://github.com/Asuswrt-Merlin/asuswrt-merlin.ng-3006.gnuton/actions/workflows/github-actions.yml)
[![Latest release](https://img.shields.io/github/v/release/Asuswrt-Merlin/asuswrt-merlin.ng-3006.gnuton?display_name=tag&sort=semver)](https://github.com/Asuswrt-Merlin/asuswrt-merlin.ng-3006.gnuton/releases)

This repository is the **3006 firmware line** of GNUton's Asuswrt-Merlin.ng
fork. It contains the source, CI workflows, firmware update feed and releases
for the supported WiFi 7 models, and is independent of the older 3004
repository.

> **Looking for 3004 / WiFi 6 builds?**
> They live in
> [Asuswrt-Merlin/asuswrt-merlin.ng-3004.gnuton](https://github.com/Asuswrt-Merlin/asuswrt-merlin.ng-3004.gnuton).
> This repository builds the 3006 line only and rejects non-`3006.*` tags.

## Supported models

| Model | Platform |
| --- | --- |
| GT-BE98 | Broadcom BCM4916 |
| RT-BE58U | Broadcom BCM676x |
| RT-BE82U | Broadcom BCM676x |
| RT-BE82M | Broadcom BCM676x |

Supported models and available images can vary between releases; check the
assets of each [release](https://github.com/Asuswrt-Merlin/asuswrt-merlin.ng-3006.gnuton/releases).

## Downloads and firmware updates

- Firmware images: <https://github.com/Asuswrt-Merlin/asuswrt-merlin.ng-3006.gnuton/releases>
- Update feed: <https://raw.githubusercontent.com/Asuswrt-Merlin/asuswrt-merlin.ng-3006.gnuton/master/updates/manifest_3006.txt>
- Release notes: [`updates/`](updates/)

The routers' built-in update check reads `updates/manifest_3006.txt` from this
repository. Stable releases update the manifest and release-note files
automatically; `updates/**` and Markdown-only commits do not trigger builds.

## About

Asuswrt-Merlin is an enhanced version of Asuswrt. The goal of the project is to
fix issues and make minor functionality adjustments while keeping the original
Asus firmware featureset. This fork adds support for additional router models
on top of [RMerl/asuswrt-merlin.ng](https://github.com/RMerl/asuswrt-merlin.ng).

## Building

The GitHub Actions workflow builds the model matrix in the GNUton toolchain
container. A single model can be built on demand from the Actions tab with
`workflow_dispatch` and the `model` input (for example `rt-be82m`); leaving it
empty builds every matrix entry.

Local builds use the usual AsusWrt-Merlin build system. For the WiFi 7 models,
run from the BE SDK tree:

```sh
cd release/src-rt-5.04behnd.4916
make gt-be98
make rt-be82m
```

## Support

Community support is available on the
[SNBForums Asuswrt-Merlin forum](https://www.snbforums.com/forums/asuswrt-merlin.42/).
