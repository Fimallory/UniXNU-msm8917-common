# AGENTS.md

Shared MSM8917 support for UniXNU device repositories.

- Keep hardware values traceable to the stock rolex DTB or the device hardware
  inventory; do not invent register addresses.
- `soc.toml` is a profile layer merged before the OS profile and the device
  `board.toml`. Only put SoC-wide defaults here.
- `dts/msm8917.dtsi` is included by device DTS files. Keep it compilable.
- `configs/msm8917.config` holds SoC kernel options; the device defconfig selects
  the board configuration.
- `patches/xnu/series` lists SoC patches in order; an empty series is valid.
