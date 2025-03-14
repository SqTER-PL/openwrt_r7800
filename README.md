![OpenWrt logo](include/logo.png)

OpenWrt Project is a Linux operating system targeting embedded devices. Instead
of trying to create a single, static firmware, OpenWrt provides a fully
writable filesystem with package management. This frees you from the
application selection and configuration provided by the vendor and allows you
to customize the device through the use of packages to suit any application.
For developers, OpenWrt is the framework to build an application without having
to build a complete firmware around it; for users this means the ability for
full customization, to use the device in ways never envisioned.

Sunshine!

## Release Info

Sources update 2025-03-14

Restored swconfig

Added NSS support for ipq806x

Disable noise for ATH10k-CT driver

## Supported QCA-NSS modules
```
kmod-nss-ifb-6.6.82-r1.apk
kmod-qca-mcs-6.6.82.2023.06.01~67832897-r1.apk
kmod-qca-nss-drv-6.6.82.2020.03.20~3cfb9f43-r2.apk
kmod-qca-nss-drv-igs-6.6.82.2020.10.29~ef082a73-r2.apk
kmod-qca-nss-drv-l2tpv2-6.6.82.2020.10.29~ef082a73-r2.apk
kmod-qca-nss-drv-netlink-6.6.82.2020.10.29~ef082a73-r2.apk
kmod-qca-nss-drv-pppoe-6.6.82.2020.10.29~ef082a73-r2.apk
kmod-qca-nss-drv-pptp-6.6.82.2020.10.29~ef082a73-r2.apk
kmod-qca-nss-drv-profile-6.6.82.2020.10.29~ef082a73-r2.apk
kmod-qca-nss-drv-qdisc-6.6.82.2020.10.29~ef082a73-r2.apk
kmod-qca-nss-drv-tun6rd-6.6.82.2020.10.29~ef082a73-r2.apk
kmod-qca-nss-drv-tunipip6-6.6.82.2020.10.29~ef082a73-r2.apk
kmod-qca-nss-ecm-standard-6.6.82.2023.07.25~9acdcb05-r1.apk
kmod-qca-nss-gmac-6.6.82.2023.06.01~17176794-r1.apk
mac80211-nss-support
```
## License

OpenWrt is licensed under GPL-2.0

