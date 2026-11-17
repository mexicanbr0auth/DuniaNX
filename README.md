# DuniaNX

> Recompiled. Reborn. Native.

![Platform](https://img.shields.io/badge/Platform-Android-3DDC84?logo=android)
![Architecture](https://img.shields.io/badge/Architecture-ARM64-blue)
![GPU](https://img.shields.io/badge/Priority-Mali-orange)
![Graphics](https://img.shields.io/badge/Vulkan-Planned-red)
![Status](https://img.shields.io/badge/Status-In_Development-yellow)

## About

DuniaNX is an experimental native Android port project
targeting the Xbox 360 version of Far Cry 2.

The project uses static recompilation to execute the
original PowerPC game code natively on ARM64 processors,
powered by a custom Xbox 360 compatibility runtime.

No Windows compatibility layer.
No traditional full-system emulation.

Built with mobile hardware in mind, prioritizing Mali GPUs.

## Current Development Status

- [x] PowerPC to ARM64 static recompilation
- [x] Over 92,000 recompiled functions
- [x] 10,223 shaders translated to HLSL
- [x] Native execution of original game code
- [x] Virtual and physical memory support
- [x] Initial Xbox 360 compatibility runtime
- [x] Access to original game resources
- [x] Direct disk I/O
- [x] Cooperative scheduler and workers
- [x] Over 9,000 kernel calls reached during startup
- [ ] Full Dunia engine initialization
- [ ] SPIR-V shader compilation
- [ ] Vulkan graphics backend
- [ ] First rendered frame
- [ ] Audio playback
- [ ] Touchscreen controls
- [ ] Playable gameplay
- [ ] Public APK release

## Frequently Asked Questions

### Does the game already run?

The original game code is already executing natively
on Android and accessing original game resources.

Full engine initialization is still in development.

### Does it render?

Not yet. The first rendered frame is one of our
next major milestones.

### Is it playable?

Not yet.

### Is this an emulator?

DuniaNX uses static recompilation and a custom
Xbox 360 compatibility runtime instead of traditional
full-system emulation.

### Will Mali GPUs be supported?

Mali GPUs are our primary development target.

### What about Adreno GPUs?

Broader Vulkan-compatible device support is planned,
but Mali remains our initial priority.

### Is there an APK available?

Not yet. There is currently no public playable build.

### Release date?

When it's ready.

## Developed on Android, for Android

The project is being developed, compiled and tested
entirely on an Android smartphone using Termux.

No desktop computer involved in the current workflow.

## Follow the Development

This repository serves as the official announcement
and development progress page for DuniaNX.

Star this repository to follow future updates,
technical milestones and release announcements.

## Disclaimer

DuniaNX is an independent, unofficial experimental
project. It is not affiliated with, endorsed by,
or sponsored by Ubisoft or Microsoft.

Far Cry 2, Dunia Engine and all related trademarks
and copyrighted materials belong to their respective
owners.

No proprietary game assets or original game
executables are distributed in this repository.

Users will need their own legally obtained copy
of the original game.

---

<div align="center">

### AFRICA. IN YOUR POCKET.

*The journey has already begun.*

**DuniaNX — Stay tuned.**

</div>
