# Newifi D2 Linux 5.10 GitHub Actions build

This project builds a **D-Team Newifi D2 (MT7621)** image from pinned
`coolsnowwolf/lede` commit `07cebbcf28f61637047645c3fedc829ee9cc3688`.
The device profile uses vendor `kmod-mt7603e`, `kmod-mt76x2e`, `mt_wifi`,
`luci-app-mtwifi`, and `kmod-mtk-hnat`. `config.seed` selects the target and
the MT7603E+MT7612E wireless profile.

Run the **Build Newifi D2 (Linux 5.10)** workflow from GitHub Actions, or
push a change to this repository. The workflow uploads the sysupgrade image
and SHA256 checksum as an artifact. On failure it uploads the build log.

The workflow checks configuration and image generation. It does **not**
validate Wi-Fi, Ethernet, NAT or configuration preservation on hardware.
Do not flash an image until the build succeeds and the image is reviewed.
