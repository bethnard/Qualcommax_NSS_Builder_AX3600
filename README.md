# Qualcommax NSS Builder

This project automates the process of building OpenWrt firmware images for the Qualcomm IPQ807x platform, specifically targeting the Xiaomi AX3600 router. The build process incorporates various optimizations, hardening options, and quality-of-life enhancements. 

## Features

- Automated build process triggered by new commits in the [remote repository](https://github.com/qosmio/openwrt-ipq) or manual workflow dispatch
- Compiler optimizations for improved performance
- Hardening build options for enhanced security
- SSH configuration with strong algorithms and key exchange methods. Refer to the [`ssh_hardening.config`](files/etc/ssh/sshd_config.d/ssh_hardening.conf)
- Additional useful packages. Refer to the [`ax3600.config`](ax3600.config)
- Full NSS (Network Subsystem) support 
- Quality-of-life enhancements through UCI configuration

## Fork 

- Adblock-Fast
- DDNS (FreeDNS)
- HTTPS-DNS-Proxy
- MWAN3 (WAN Load Balance) + all necessary IPTable adjustments
- Nano
- TTYD
- Watchcat
- Wake-on-LAN
- Wireguard
- And more

## Build Process

The automated build process is broken! Build is done through own compilation:

1. Clone this and qosmio's NSS repository:
   ```bash
   git clone https://github.com/bethnard/Qualcommax_NSS_Builder_AX3600 -b main --single-branch
   git clone https://github.com/qosmio/openwrt-ipq -b main-nss --single-branch
   cd openwrt-ipq
   ```
2. Update feeds:
   ```bash
   ./scripts/feeds update
   ./scripts/feeds install -a
   ```
3. Copy AX3600.config file
   ```bash
   cd ..
   cp Qualcommax_NSS_Builder_AX3600/ax3600.config openwrt-ipq/.config
   cp -r Qualcommax_NSS_Builder_AX3600/files/ openwrt-ipq/
   cd openwrt-ipq
   ```
4. Generate the full config
   ```bash
   make defconfig V=s
   ```
5. Now run full build
   ```bash
   make download -j$(nproc) V=s
   make -j$(nproc) V=s

## Configuration

The project utilizes a custom configuration file [`ax3600.config`](ax3600.config) to specify the desired settings for the firmware build. This file includes various options such as target platform, compiler optimizations, package selections, and more.

Additionally, the `uci` commands in the "Quality-of-Life Enhancements" section are used to fine-tune the wireless and network settings for improved performance and functionality. Refer to the [999-QOL_config](https://github.com/bethnard/Qualcommax_NSS_Builder/tree/main/files/etc/uci-defaults/999-QOL_config) for the specific configuration. 

## SSH Hardening

To enhance the security of SSH connections, the project includes a hardened SSH configuration. The configuration is derived from recommendations by [SSH-Audit](https://github.com/jtesta/ssh-audit) and the [BSI](https://www.bsi.bund.de/), it specifies strong key exchange algorithms, ciphers, message authentication codes (MACs), host key algorithms, and public key algorithms. This ensures that only secure and up-to-date algorithms are used for SSH communication.

## Contributing

Contributions to this project are welcome. If you encounter any issues or have suggestions for improvements, please open an issue or submit a pull request on the GitHub repository.

## Acknowledgements

- The OpenWrt project for providing the foundation for this firmware build.
- The Qualcomm IPQ807x platform and the Xiaomi AX3600 router for the hardware support.
- The community over at the [OpenWrt forum](https://forum.openwrt.org/t/ipq807x-nss-build/148529) for their valuable contributions and resources. 
- [rodriguezst](https://github.com/rodriguezst) for his [ipq807x-openwrt-builder](https://github.com/rodriguezst/ipq807x-openwrt-builder)
- A special thanks to [qosmio](https://github.com/qosmio) for the main NSS development
- And [JuliusBairaktaris](https://github.com/JuliusBairaktaris) for his amazing project / repo!
