# AGENTS.md

Instructions for coding agents, and the people driving them, working in malachite's
prebuilt kernel repository. The general rules (branches, commits, license headers,
testing) are in the
[device tree's AGENTS.md](https://github.com/malachite-project/android_device_xiaomi_malachite/blob/lineage-23.2/AGENTS.md)
and apply here too, including working as an official LineageOS maintainer would: fix
causes properly, never with workarounds.

## What this repository is

The kernel the device tree ships: `Image.lz4`, the DTBs, the module directories and the
`modules.load.*` lists, consumed at `device/xiaomi/malachite-kernel`. It is built from the
kernel sources in [kernel_manifest-6.1](https://github.com/malachite-project/kernel_manifest-6.1),
not edited here.

## Rules

- **Never edit artifacts by hand.** A new kernel or module comes from a build of the
  kernel sources. The commit body names the source commits, the build command and the
  config used.
- **Never copy a whole build output over this repository.** Compare it with what is here
  first: configuration, module list, vermagic, exported symbols, signing, load order.
  Replace only what you mean to replace.
- **Keep the module load order.** Don't reorder or remove `modules.load.*` entries just
  to silence a warning; the order is what makes the phone boot.
- **DTBO:** `dtbo.img` is kept for reference only. The device tree doesn't flash it,
  because flashing the wrong DTBO across firmware versions has bricked phones. Never make
  it flashed again without an explicit request.
- **Never run flashing commands**, and never suggest downgrading or cross-flashing
  firmware to match a kernel.

## Checking artifacts

```
python3 tools/inspect_prebuilts.py
```

The inspector checks hashes, headers and load lists. It doesn't prove a kernel boots;
say so when reporting.

## Commits

```
malachite-kernel: update modules to <source commit>
malachite-kernel: tools: recognize the Android DT-table container
```
