# AGENTS.md - AsusWrt-Merlin.ng (3006)

This repository is the **3006 firmware line** of GNUton's AsusWrt-Merlin.ng
fork. It is self-contained: sources, CI, the firmware update feed and releases
all live here. The older `Asuswrt-Merlin/asuswrt-merlin.ng-3004.gnuton` repository
is the **3004** line and must not be modified as part of work in this repo.

## Git Workflow & Branches
- **Upstream**: [RMerl/asuswrt-merlin.ng](https://github.com/RMerl/asuswrt-merlin.ng). Data from upstream branches or tags is merged into origin branches.
- **Origin**: `Asuswrt-Merlin/asuswrt-merlin.ng-3006.gnuton`, the target for pushing changes.
- **Firmware line**: `master` is the 3006 line. There is no `master-3006` branch here.
- **Version format**: Versions and tags follow `3006.102.9_1-gnuton1`.
- **Tags**: Only `3006.*` tags are built. CI fails before building any other tag (in particular 3004 tags).

## Supported Models (via GitHub Actions)
- `gt-be98` (GT-BE98, default and ROG UI)
- `rt-be58u` (RT-BE58U)
- `rt-be82u` (RT-BE82U)
- `rt-be82m` (RT-BE82M)

3004/WiFi 6 models are supported by the separate
`Asuswrt-Merlin/asuswrt-merlin.ng-3004.gnuton` repository, not here.

## Firmware Update Feed
- Feed URL used by `release/src/router/rom/webs_scripts/merlin_webs_update.sh`:
  `https://raw.githubusercontent.com/Asuswrt-Merlin/asuswrt-merlin.ng-3006.gnuton/master/updates`
- Manifest read by the updater: `updates/manifest_3006.txt`, one line per model:
  `<MODEL>#FW3006.102.9#EXT1-gnuton1`.
- Release notes: `updates/3006_102.9_1-gnuton1_note.txt` (dots in the version are
  kept; the first dot after `3006` becomes an underscore).
- Do not copy 3004 manifests or release notes into this repository.
- Pushes that only touch `updates/**` or Markdown files do not trigger builds
  (`paths-ignore`). `[SKIP_CI]` in the manifest commit message is a convention
  only; the workflow does not parse it.

## Architecture & Structure
- `release/src/`: Main source tree for the firmware.
- `release/src-rt-*/`: SDK trees (e.g. `src-rt-5.04behnd.4916` for the BE models).
- `www/`: Web user interface files.
- `scripts/` and `tools/`: Build and helper utilities.
- `updates/`: Firmware update feed served from the `master` branch.

## Build System
The build system uses a complex set of Makefiles. For this line the top-level
image build is driven by `release/src-rt-5.04behnd.4916/Makefile`, with the
router userland under `release/src/Makefile`.

### Key Build Commands
Run from `release/src-rt-5.04behnd.4916/` (the SDK tree used by the BE models):
- `make [model_id]`: Builds the image for a specific model.
- `make mk-[package]`: Builds a specific package.
- `make distclean`: Cleans all configurations and builds.
- `make cleanimage`: Removes generated images in `image/`.
- `make cleankernel`: Performs a `distclean` of the Linux kernel while preserving `.config`.

### Build Process Notes
- The build process involves copying "base" config files to "target" config files and then applying modifications based on the target profile.
- Trust the `Makefile` targets and logic over prose documentation for the exact build sequence.

## CI
- Workflow: `.github/workflows/github-actions.yml`.
- A generic `PROJECT_DIR` (`/project/repo`) is cloned from the event's repository;
  do not reintroduce repository-name-specific paths such as
  `/home/runner/work/asuswrt-merlin.ng/...`.
- `workflow_dispatch` accepts a `model` input; leave it empty to build all models.
- `release-job` and `manifest-job` run only for tag **push** events, never for a
  manual `workflow_dispatch` on a tag. The manifest job also waits for the
  release job, so the feed is not updated if release creation fails.
- The manifest job only advertises models whose default artifact exists in the
  tag's build, and updates `updates/manifest_3006.txt` idempotently.

## Development Constraints
- **Proprietary Code**: Proprietary components from ASUSTeK, Broadcom, Trend Micro, and Tuxera are present. These are licensed ONLY for original ASUSTeK devices.
- **Prebuilt Components**: Many components in `prebuilt` directories are precompiled binaries. Source code for these is not available; changes requiring modifications to these binaries cannot be implemented.
- **Style**: Follow existing C and shell script conventions found in the `release/src/router` and `release/src-rt-*` directories. Avoid introducing modern styles that might break compatibility with the legacy toolchain.

## Verification
- There is no single global test suite (like `npm test`). Verification is typically performed by building the image and flashing it to hardware or using specific package build targets (`mk-[package]`).
- Use `nvram get productid` via SSH on a physical router to verify model versions.
