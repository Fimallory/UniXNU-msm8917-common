# UniXNU-msm8917-common

Shared MSM8917 support for UniXNU device repositories. This is a scaffold created
for the Redmi 4A (rolex) port.

Current contents:

- `dts/msm8917.dtsi`: minimal SoC description (CPUs, PSCI, GIC-400, ARMv8 timer)
  from the board hardware inventory. Peripheral nodes are still missing.
- `configs/msm8917.config`: disables ARMv8.1 PAN on Cortex-A53 and selects
  architectural timer entry.
- `patches/xnu/series`: empty patch list.

Not yet implemented: platform operations, SMMU, clocks, storage, display, USB,
interrupt remapping quirks, and any XNU driver patches. Treat every value as a
scaffold until it is checked against hardware.

Kernel phase1 corrects the acquired CPU affinities to0x100-0x103 and exports
GICD/GICC and timer to the Apple-format tree via `unixnu,adt-name`. The rolex
board disables secondary CPUs for the initial single-core port. The two
GICv2 MMIO ranges are the acquired distributor/CPU interface, not invented
redistributor or virtualization windows. Driver compilation and runtime
interrupt support remain separate milestones.
