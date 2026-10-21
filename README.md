# MT7621 OpenWrt build (Newifi D2)

This project builds a D-Team Newifi D2 image from the **official OpenWrt
24.10** source at commit `96922957ff073a74afbc15adbbd5e978f522fd29`.
The upstream device definition uses the open-source MT7603/MT76x2 Wi-Fi
drivers on Linux 6.6. The build does not fetch LEDE or apply its vendor
Wi-Fi/HNAT patch.

Run **Build Newifi D2 (official OpenWrt 24.10)** on GitHub Actions. The
workflow checks selected packages, builds the sysupgrade image and uploads
it with a SHA256 checksum. It does not verify Wi-Fi, Ethernet, NAT or
configuration preservation on hardware.

The image selects firewall4 and nftables flow offloading. The earlier
LEDE-only Turbo ACC and nft full-cone packages are not included. The
firewall UI can configure software/hardware flow offloading where supported;
hardware acceleration needs validation on the device.

The software selection includes ddnsto, SmartDNS, socat, ZeroTier, their
available LuCI pages, Chinese translations, Argon and a local
`default-settings` package. This defaults package sets the LuCI language
to Chinese and theme to Argon at first boot. The custom ZeroTier LuCI page
retains the LAN-to-ZeroTier forwarding switch from the CMCC3000 build.

The public build does not include the router's credentials or ZeroTier
identity. A separate private repository supplies the ddnsto token and
ZeroTier network ID through an Actions secret; it generates a new ZeroTier
identity after flashing. The old firewall configuration is not copied.
