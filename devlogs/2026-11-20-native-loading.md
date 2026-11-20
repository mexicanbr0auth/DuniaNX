# DuniaNX — Native Loading and Vulkan Integration Log

**Date:** November 20, 2026

**Platform:** Android ARM64, Termux / Termux:X11, Mali-G57 MC2

**State:** Technical research and native proof-of-execution, **not playable**

## What actually runs

The original Xbox 360 Far Cry 2 title is statically recompiled from
PowerPC to native ARM64 and runs inside a partial Xbox 360 compatibility
runtime. A restricted translator interprets original Xenos GPU command
buffers and submits corresponding work through a **real Vulkan backend**
on the Mali-G57 MC2 GPU. The source image is the game's **actual guest
frontbuffer**. The development viewer presents a captured copy in X11;
it does not integrate the game's rendering loop directly with an Android
Vulkan surface. It is not a replacement splash screen or mock UI.

The loading indicator and startup UI artwork are visible and change as
execution proceeds. **Main menu, gameplay, audio output and a complete
native Android presentation path have not been demonstrated.** The prior
private signed v0.6 Android APK is a separate, older diagnostic milestone.

## Concrete boot results

| Boot | Observed result | Outcome |
|:---|:---|:---|
| 263 | 100 frontbuffer snapshots; 38 distinct hashes; 9,458 BC3 draws | Watchdog, no newly confirmed PM4 stop |
| 264 | 216 frontbuffer snapshots; 147 distinct hashes; 20,592 BC3 draws; 2,624,922 imports | Nine-minute watchdog, main menu not reached |
| 265 | 1 snapshot; 2,556,397 imports; guest thread 10 stopped advancing between imports | Manually terminated after several minutes without progress |
| 266 | **247 snapshots; 158 distinct hashes; 23,520 BC3 draws; 2,630,893 imports** | Nine-minute watchdog, authentic loading animation continued; menu not reached |

**Boot 267** was launched afterward to evaluate the optional exhaustive
row-based Vulkan readback path in a real native guest execution. Its outcome
is pending; do not infer performance from the earlier diagnostic tests.

A **snapshot** is a real guest-memory capture, not evidence of an Android
native swapchain present. Hashes indicate differing captured pixel bytes,
not unique gameplay scenes. Import counts are repeated calls, not unique
implementations, elapsed game seconds, or frame-rate measurements.

## Original UI shaders verified

- Recovered an original captured UI vertex/pixel shader pair and compiled
  it to validated SPIR-V for the restricted native Vulkan path.
- In boot **263**, ran both the old restricted UI renderer and the original
  translated shader pair for **16 actual captured UI draws**.
- **68,252 of 68,252 compared pixels matched exactly.** These are real
  GPU readbacks from the selected cases, not a test of all 10,223 shaders.
- Further pipeline/state coverage, arbitrary textures and shaders remain
  incomplete. The previous renderer remains the main output path.

## Diagnosing loading throughput

The guest engine advances through Xenos graphics command submission,
`VdSwap`, EDRAM resolves and tiled frontbuffer capture, but the loading
phase continues reporting `update_mask=0xC00`, with its observed display
fields `display24=0` and `display28=0`. This does not prove a specific
Dunia initialization defect; more engine diagnostics are required.

- Optional cooperative scheduler quantum scaling (`DUNIANX_SCHED_QUANTUM_FACTOR`)
  and lock-owner profiling were added without forcing locks open.
- CPU units sampled in the native loader were compiled individually at
  `-O1`, without fast-math or modifications to guest control flow.
- Boot **265** demonstrated that native optimization alone is not enough:
  it remained between two imports while consuming CPU. This is recorded
  as a regression, not a success claim.
- Optional host thread labels (`DUNIANX_HOST_THREAD_NAMES`) associate Xbox
  threads 9, 10 and 16 with Android TIDs.
- An opt-in, nonfatal **SIGUSR2 native PC sampler** (`DUNIANX_NATIVE_PC_SAMPLE`)
  can map ARM64 PCs back to symbols from the *running* executable. A guarded
  helper refuses sampling unless the target is the matching running boot
  process with the handler enabled.
- Boot **266** passed the 265 stall point. Actual samples hit
  `renderTexturedRectangle`, `renderColorBands`, `renderZeroRectangle`,
  `snapshotFrontbuffer` and libc wait functions. This demonstrates concrete
  render/readback work, not a fully solved engine-load bottleneck.

## GPU performance experiments and safeguards

- Optional byte-exact BC3 texture cache (8-entry LRU) reuses identical decoded
  texture data while respecting the original tiled texture contents.
- Optional persistent Vulkan UI pipeline resources reuse shader modules,
  pipeline, render pass and descriptors; native device cleanup is explicit.
  A 48-draw microbenchmark produced only a modest benefit; no game FPS
  improvement is claimed from it.
- Optional skipping of proven-redundant idle GPU sync submits still retains
  real command fences and readbacks for draws.
- `DUNIANX_FAST_GPU_READBACK_VERIFY=1` replaces per-component verification
  loops with accelerated **whole-row comparisons** in color-band, textured
  rectangle and depth rectangle renderers. Every output byte is checked;
  no frame content is replaced, and mismatch diagnostics retain exact bytes.
  The legacy verification path remains available with the option disabled.
- These optimized and baseline paths independently passed **11,293,560**
  rectangle/depth checks and **923,052** color-band checks with Vulkan.
  Additional targeted tests passed: **602,012** recompiled parser checks,
  **6,451** runtime checks and **206** cooperative threading checks.
  These are test assertions, not rendered frames or a full post-change
  regression-suite claim.

## Next milestone

Finish the actual Dunia loading transition and render its **real main menu**.
Investigate guest worker progress and remaining unsupported GPU states;
verify native renderer changes with non-regression tests. An integrated
Android Vulkan presentation surface, touch/controller input, playable
performance, sound and general device compatibility are later goals.

No proprietary game files, original game code, private APK binaries,
shader dumps or large artifacts are included in this public devlog.
