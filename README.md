# UniXNU-msm8917-common

Shared MSM8917 support for UniXNU device repositories. This is a scaffold created
for the Redmi 4A (rolex) port.

Current contents:

- `dts/msm8917.dtsi`: minimal SoC description (CPUs, PSCI, GIC-400, ARMv8 timer)
  from the board hardware inventory. Peripheral nodes are still missing.
- `configs/msm8917.config`: kernel configuration fragment; no SoC driver selected.
- `patches/xnu/series`: empty patch list.

Not yet implemented: platform operations, SMMU, clocks, storage, display, USB,
interrupt remapping quirks, and any XNU driver patches. Treat every value as a
scaffold until it is checked against hardware.
