# UniXNU-msm8917-common

Shared MSM8917 support for UniXNU device repositories. This is a scaffold created
for the Redmi 4A (rolex) port.

Current contents:

- `dts/msm8917.dtsi`: minimal SoC description (CPUs, PSCI, GIC-400, ARMv8 timer)
  from the board hardware inventory. Peripheral nodes are still missing.
- `configs/msm8917.config`: disables ARMv8.1 PAN on Cortex-A53 and selects
  architectural timer entry and validated DT panic-region discovery.
- `patches/xnu/series`: Cortex-A53 SCTLR RES1 correction for the BCM2837
  source build configuration used by this port.

Not yet implemented: platform operations, SMMU, clocks, storage, display, USB,
interrupt remapping quirks, and any XNU driver patches. Treat every value as a
scaffold until it is checked against hardware.

Kernel phase1 corrects the acquired CPU affinities to0x100-0x103 and exports
GICD/GICC and timer to the Apple-format tree via `unixnu,adt-name`. The rolex
board disables secondary CPUs for the initial single-core port. The two
GICv2 MMIO ranges are the acquired distributor/CPU interface, not invented
redistributor or virtualization windows. Driver compilation and runtime
interrupt support remain separate milestones.

The pinned XNU default SCTLR0x3454593d omits bit23, which is RES1 on
Cortex-A53 without PAN. The SoC patch adds only that bit to SCTLR_RESERVED
for BCM2837, producing0x34d4593d while keeping all other flags unchanged.
This is an architectural write-policy correction, not proof of the observed
pre/post-MMU stopping cause. TCR0x22a51a251a selects4KiB pages,40-bit PA and
38-bit VA, which are supported by Cortex-A53. Source reference:
https://support.arm.com/documentation/ddi0500/j/System-Control/AArch64-register-descriptions/System-Control-Register--EL1

The BCM2837 personality supplies Pi-specific panic-log PA0x3c0fc000, outside
the acquired MSM8917 DRAM inventory. `CONFIG_XNU_DT_PANIC_REGION=y` selects
the existing native-dt-panic-region patch's bounded DT discovery. No `pram`
input means the optional old-panic probe returns without memory access; actual
boot-video console initialization is retained. This is not an unconditional
console/platform bypass or proof of the currently observed stopped instruction.
