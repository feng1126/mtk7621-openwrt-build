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

## Software matched to the current router

The package list follows the installed software on Newifi D2 at
`192.168.10.20`: vendor Wi-Fi, HNAT, ddnsto, SmartDNS, socat, ZeroTier,
their available LuCI applications, Chinese LuCI translations, Argon,
block mounting and 6in4. The ddnsto and socat LuCI packages are included
in `local_packages/`; the first-boot defaults select Argon and Chinese.
The ZeroTier LuCI package also comes from CMCC3000 and includes its
LAN-to-ZeroTier forwarding switch.

The router's live configuration contains credentials, a ZeroTier identity,
and network-specific settings. These are **not committed or embedded in the
public firmware**. A private sysupgrade backup was taken separately for
restoration after installing a verified image. The build intentionally does
not copy the existing firewall configuration because this LEDE source uses
firewall4 while the running device has legacy firewall packages.

## Reference implementation

[`feng1126/openwrt`](https://github.com/feng1126/openwrt) at
`aeb2d7681e821d53c305865f869c583dade63959` is another Linux 5.10
MT7621/HNAT port. It is kept as a reference because its runtime stability
is unconfirmed. It uses the `mediateksdk` Ethernet/HNAT driver,
`kmod-ramips_hnat`, and additional network-path patches. This build uses
the pinned LEDE source's `mtk` Ethernet/HNAT integration and
`kmod-mtk-hnat`; copying only the reference HNAT module would leave its
kernel and Ethernet dependencies unresolved. The reference Newifi D2
profile also selects the open-source MT76 Wi-Fi packages by default, while
this build explicitly selects the vendor Wi-Fi packages and profile.
