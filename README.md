<p align="center">
  <img src=".github/assets/root-my-galaxy-payloads-banner.svg" alt="Root My Galaxy Payloads Extended" width="100%" />
</p>

<p align="center">
  <a href="https://github.com/igorcv88/Root-My-Galaxy-Extended/releases/latest"><img alt="App release" src="https://img.shields.io/github/v/release/igorcv88/Root-My-Galaxy-Extended?label=app" /></a>
  <img alt="Android" src="https://img.shields.io/badge/Android-16%20%2F%2017-3DDC84?logo=android&amp;logoColor=white" />
  <img alt="KernelSU" src="https://img.shields.io/badge/KernelSU-3.3.0-2f81f7" />
  <a href="https://github.com/igorcv88/Root-My-Galaxy-Payloads-Extended/actions/workflows/update-payloads.yml"><img alt="Payload build" src="https://img.shields.io/github/actions/workflow/status/igorcv88/Root-My-Galaxy-Payloads-Extended/update-payloads.yml?branch=main&amp;label=payloads" /></a>
  <a href="LICENSE"><img alt="License" src="https://img.shields.io/github/license/igorcv88/Root-My-Galaxy-Payloads-Extended" /></a>
</p>

<p align="center"><strong>Firmware profiles, exploit payloads, root helper and Samsung KernelSU artifacts used by Root My Galaxy Extended.</strong></p>

<p align="center">
  <a href="https://github.com/igorcv88/Root-My-Galaxy-Extended">App</a> ·
  <a href="https://github.com/BuSung-dev/Root-My-Galaxy-Payloads">Upstream</a> ·
  <a href="support/targets-v3.json">Current feed</a>
</p>

## About

This repository maintains the firmware-specific files consumed by [Root My Galaxy Extended](https://github.com/igorcv88/Root-My-Galaxy-Extended). Each supported profile describes the exact device/build/kernel identity and the matching exploit, KernelSU and root-helper artifacts.

> [!WARNING]
> These payloads use a kernel exploit. A failed attempt can reboot or panic the device. Firmware and kernel updates can require a new exact profile.

## Current profiles

| Device | Firmware | Android | Kernel | Status |
| --- | --- | --- | --- | --- |
| Galaxy S25 Ultra SM-S938B | `S938BXXSBCZG3` | 16 | 6.6.98 | Supported |
| Galaxy S25 Edge SM-S937B | `S937BXXU8CZF1` | 16 | 6.6.98 | Verified exact-match support |
| Galaxy S25 Ultra SM-S938B | `S938BXXUCZZI4` | 17 / One UI 9 beta | 6.6.127 | Hardware validated |
| Galaxy S25 Ultra SM-S938B | `S938BXXUCDZIF` | 17 / One UI 9 | 6.6.127 | Hardware validated |
| Galaxy S25 SM-S931B | `S931BXXUCZZI4` | 17 / One UI 9 beta | 6.6.127 | Exact-match profile available |
| Galaxy S25+ SM-S936B | compatible 6.6.98 builds | 16 | 6.6.98 | Advanced support (unvalidated) |

The feed also retains a legacy generic Galaxy S25/S25+ kernel-6.6.98 entry. Current exact identities are published in [support/targets-v3.json](support/targets-v3.json).

The Galaxy S25 Edge `SM-S937B / S937BXXU8CZF1` profile is an exact match for codename `psq`, Android 16, kernel release `6.6.98-android15-8-pd6ff1cd-abogkiS937BXXU8CZF1-4k`, arm64-v8a and 4 KiB pages. SM-S936B currently has **Advanced support (unvalidated)** through the generic S25/S25+ kernel-6.6.98 profile. Automatic selection still requires an exact profile.

## Feed and artifact verification

`support/targets-v3.json` is the app-facing manifest. Exact profiles include device/build/kernel identity, artifact URLs, sizes, SHA-256 hashes and the target's exploit route policy.

The app verifies these values before execution. Firmware-specific files stay in separate artifact paths so an update for one target does not replace another target's binaries.

## KernelSU and DEFEX

The maintained KernelSU 3.3.0 builds include Samsung-specific KDP, RKP and DEFEX handling.

On the supported One UI 9 path, the verified `ksud` is staged before late-load. The root helper uses a DEFEX-compatible bind execution path, then KernelSU completes its daemon, mount and module lifecycle from the correct namespace. This provides the KernelSU base used by Zygisk Next and LSPosed after root.

ZZI4 uses a firmware-specific KernelSU pair and staged-daemon fix. Generic CI does not rebuild that pair automatically.

See [kernelsu/README.md](kernelsu/README.md) for build and late-load details.

## ZZI4 payload

The S938B ZZI4 payload was derived for its 6.6.127 kernel and validated on hardware. It uses Tracefs for KASLR discovery when available and the physical-load alias for the validated kernel data-write path.

Target constants and policy remain firmware-specific. The S931B ZZI4 profile has its own exact identity and artifacts.

Technical derivation belongs in [docs/PORTING.md](docs/PORTING.md) and the target source directories.

## Publication

The payload workflow builds declared targets from one pinned source commit and checks that `main` has not advanced before publishing generated artifacts.

Known-good firmware-specific KernelSU binaries can be preserved while the exploit or metadata for a target is updated. Existing historical artifact paths remain separate from current versioned paths.

## Documentation

- [Support feed and matching rules](support/README.md)
- [Firmware/profile porting](docs/PORTING.md)
- [KernelSU builds and late-load](kernelsu/README.md)
- [Production architecture notes](docs/ARCHITECTURE.md)

## Credits

This repository uses or adapts work from:

- [BuSung-dev/Root-My-Galaxy-Payloads](https://github.com/BuSung-dev/Root-My-Galaxy-Payloads)
- [BuSung-dev/Root-My-Galaxy](https://github.com/BuSung-dev/Root-My-Galaxy)
- [mitschud](https://github.com/mitschud) and [upstream PR #300](https://github.com/BuSung-dev/Root-My-Galaxy-Payloads/pull/300)
- [NebuSec/CyberMeowfia](https://github.com/NebuSec/CyberMeowfia/tree/main/IonStack/CVE-2026-43499/exploit)
- [KernelSU](https://github.com/tiann/KernelSU)
- [HyperRamzey/Root-My-Galaxy](https://github.com/HyperRamzey/Root-My-Galaxy)

See [LICENSE](LICENSE) and the upstream projects for their respective license and copyright terms.
