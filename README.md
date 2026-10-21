# MT7621 OpenWrt build (Newifi D2)

This project builds a D-Team Newifi D2 image from **official OpenWrt
22.03** at commit `4e1d1b7df0ce6fa96d7462dc883917682f428046`.
That branch uses Linux 5.10 on MT7621. The MT7603E and MT7612E vendor
Wi-Fi driver sources are vendored under `local_packages/` from the prior
LEDE port, but the base source is only `openwrt/openwrt`. The device image
patch replaces the stock MT76 Wi-Fi packages with the vendor packages.

Run **Build Newifi D2 (official OpenWrt 22.03, Linux 5.10)** in GitHub
Actions. The workflow checks selected packages, builds the sysupgrade image
and uploads it with a SHA256 checksum. It does not verify Wi-Fi, Ethernet,
NAT or configuration preservation on hardware. Vendor Wi-Fi compatibility
with this official source must be established by successful compilation and
device testing before flashing.

The image selects firewall4, nftables flow offloading, the LEDE nft-fullcone
kernel/userspace port, and a LuCI Turbo ACC page adapted for firewall4. IPv4
full-cone NAT is controlled by the Turbo ACC switch and is off by default.
The main branch uses official MediaTek PPE hardware flow offloading for MT7621;
Turbo ACC exposes this through the flow-offloading hardware switch. On main, the LEDE
`mtkhnat.ko` driver is not included: it replaces the official MT7621
Ethernet/switch stack and Newifi D2 device tree, so a module-only transplant
would break LAN/WAN. Turbo ACC only shows the separate MediaTek HNAT option
when that module is present. The new port still needs a successful Actions build and
on-device validation before flashing.

The software selection includes ddnsto, SmartDNS, socat, ZeroTier, their
available LuCI pages, Chinese translations, Argon and a local
`default-settings` package. This package sets the LuCI language to Chinese
and theme to Argon at first boot. The custom ZeroTier LuCI page retains
the LAN-to-ZeroTier forwarding switch from the CMCC3000 build.

The public build does not include the router's credentials or ZeroTier
identity. A separate private repository supplies the ddnsto token and
ZeroTier network ID through an Actions secret; it generates a new ZeroTier
identity after flashing. The old firewall configuration is not copied.


## SDK HNAT experimental branch

The `hnat-experimental` branch applies the MediaTek SDK Ethernet/HNAT stack
from `feng1126/openwrt` to the pinned official 22.03 source, then adds Lean's
link-down FOE table cleanup. It retains the vendor MT7603E/MT7612E Wi-Fi
packages and Turbo ACC controls. The MT753X switch uses swconfig, while the
main branch uses DSA. The experimental image has compatibility version 2.0
to prevent keeping a DSA network configuration through sysupgrade. It is an
artifact for serial-console/LAN/WAN/PPPoE validation; compilation alone does
not establish that the ports or network work. The main branch remains DSA.
