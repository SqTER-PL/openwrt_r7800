# NHSS.QSDK 11.2 - 12.2
# by SqTER

include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/kernel.mk

PKG_NAME:=qca-nss-drv
PKG_RELEASE:=$(AUTORELEASE)

PKG_SOURCE_URL:=https://git.codelinaro.org/clo/qsdk/oss/lklm/nss-drv.git
PKG_SOURCE_PROTO:=git
PKG_SOURCE_DATE:=2020-03-20
PKG_SOURCE_VERSION:=3cfb9f43
PKG_MIRROR_HASH:=37d268b4712db049df3780ce298abe0456f6478a9ec2504eb303a6f10e03c818
PKG_BUILD_PARALLEL:=1

PKG_CONFIG_DEPENDS:= \
	CONFIG_NSS_DRV_BRIDGE_ENABLE \
	CONFIG_NSS_DRV_C2C_ENABLE \
	CONFIG_NSS_DRV_CAPWAP_ENABLE \
	CONFIG_NSS_DRV_CLMAP_ENABLE \
	CONFIG_NSS_DRV_CRYPTO_ENABLE \
	CONFIG_NSS_DRV_DMA_ENABLE \
	CONFIG_NSS_DRV_EDMA_ENABLE \
	CONFIG_NSS_DRV_GRE_ENABLE \
	CONFIG_NSS_DRV_GRE_REDIR_ENABLE \
	CONFIG_NSS_DRV_GRE_TUNNEL_ENABLE \
	CONFIG_NSS_DRV_IGS_ENABLE \
	CONFIG_NSS_DRV_IPV4_REASM_ENABLE \
	CONFIG_NSS_DRV_IPV6_ENABLE \
	CONFIG_NSS_DRV_IPV6_REASM_ENABLE \
	CONFIG_NSS_DRV_L2TP_ENABLE \
	CONFIG_NSS_DRV_LAG_ENABLE \
	CONFIG_NSS_DRV_MAPT_ENABLE \
	CONFIG_NSS_DRV_MATCH_ENABLE \
	CONFIG_NSS_DRV_MIRROR_ENABLE \
	CONFIG_NSS_DRV_OAM_ENABLE \
	CONFIG_NSS_DRV_PORTID_ENABLE \
	CONFIG_NSS_DRV_PPE_ENABLE \
	CONFIG_NSS_DRV_PPPOE_ENABLE \
	CONFIG_NSS_DRV_PPTP_ENABLE \
	CONFIG_NSS_DRV_PVXLAN_ENABLE \
	CONFIG_NSS_DRV_QRFS_ENABLE \
	CONFIG_NSS_DRV_QVPN_ENABLE \
	CONFIG_NSS_DRV_RMNET_ENABLE \
	CONFIG_NSS_DRV_SHAPER_ENABLE \
	CONFIG_NSS_DRV_SJACK_ENABLE \
	CONFIG_NSS_DRV_TRUSTSEC_ENABLE \
	CONFIG_NSS_DRV_TSTAMP_ENABLE \
	CONFIG_NSS_DRV_TUN6RD_ENABLE \
	CONFIG_NSS_DRV_TUNIPIP6_ENABLE \
	CONFIG_NSS_DRV_VIRT_IF_ENABLE \
	CONFIG_NSS_DRV_VLAN_ENABLE \
	CONFIG_NSS_DRV_VXLAN_ENABLE \
	CONFIG_NSS_DRV_WIFI_ENABLE

include $(INCLUDE_DIR)/package.mk

define KernelPackage/qca-nss-drv
  SECTION:=kernel
  CATEGORY:=Kernel modules
  SUBMENU:=Network Devices
  DEPENDS:=@TARGET_ipq806x||TARGET_ipq_ipq806x||TARGET_ipq_ipq807x||TARGET_ipq_ipq807x_64||TARGET_ipq807x||TARGET_ipq807x_64||TARGET_ipq_ipq60xx||TARGET_ipq_ipq60xx_64||TARGET_ipq_ipq50xx||TARGET_ipq_ipq50xx_64 \
		+kmod-qca-nss-gmac
  TITLE:=Kernel driver for NSS (core driver)
  FILES:=$(PKG_BUILD_DIR)/qca-nss-drv.ko
  AUTOLOAD:=$(call AutoLoad,32,qca-nss-drv,1)
endef

define KernelPackage/qca-nss-drv/config
	source "$(SOURCE)/Config.in"
endef

define KernelPackage/qca-nss-drv/install
	$(INSTALL_DIR) $(1)/sbin
	$(INSTALL_DIR) $(1)/lib/debug
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_DIR) $(1)/etc/sysctl.d
	$(INSTALL_DIR) $(1)/etc/hotplug.d/firmware
	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_DIR) $(1)/lib/firmware

	$(INSTALL_BIN) ./files/qca-nss-drv.sysdebug $(1)/sbin/sysdebug
	$(INSTALL_BIN) ./files/qca-nss-drv.debug $(1)/lib/debug/qca-nss-drv
	$(INSTALL_BIN) ./files/qca-nss-drv.init $(1)/etc/init.d/qca-nss-drv
	$(INSTALL_DATA) ./files/qca-nss-drv.sysctl $(1)/etc/sysctl.d/99-qca-nss-drv.conf
	$(INSTALL_BIN) ./files/qca-nss-drv.hotplug $(1)/etc/hotplug.d/firmware/10-qca-nss-fw
	$(INSTALL_DATA) ./files/qca-nss-drv.conf $(1)/etc/config/nss
	$(INSTALL_DATA) ./files/nss-firmware/qca-nss0-retail.bin $(1)/lib/firmware/qca-nss0.bin
	$(INSTALL_DATA) ./files/nss-firmware/qca-nss1-retail.bin $(1)/lib/firmware/qca-nss1.bin

endef

define KernelPackage/qca-nss-drv/Description
This package contains a NSS driver for QCA chipset
endef

define Build/InstallDev
	mkdir -p $(1)/usr/include/qca-nss-drv
	$(CP) $(PKG_BUILD_DIR)/exports/* $(1)/usr/include/qca-nss-drv/
ifneq (, $(findstring $(subtarget), "ipq807x" "ipq807x_64" "ipq60xx" "ipq60xx_64" "ipq50xx" "ipq50xx_64"))
	$(INSTALL_DIR) $(1)/usr/include/qca-nss-clients
endif
endef

EXTRA_CFLAGS+= -I$(STAGING_DIR)/usr/include/qca-nss-gmac -I$(STAGING_DIR)/usr/include/qca-nss-dp

# Keeping default as ipq806x for branches that does not have subtarget framework
ifeq ($(CONFIG_TARGET_ipq),y)
subtarget:=$(SUBTARGET)
else
subtarget:=$(CONFIG_TARGET_BOARD)
endif

ifeq ($(CONFIG_KERNEL_IPQ_MEM_PROFILE),256)
EXTRA_CFLAGS+= -DNSS_MEM_PROFILE_LOW
endif

ifeq ($(CONFIG_KERNEL_IPQ_MEM_PROFILE),512)
EXTRA_CFLAGS+= -DNSS_MEM_PROFILE_MEDIUM
endif

ifeq ($(CONFIG_KERNEL_SKB_FIXED_SIZE_2K),y)
EXTRA_CFLAGS+= -DNSS_SKB_FIXED_SIZE_2K
endif

ifeq ($(CONFIG_TARGET_BOARD), "ipq807x")
    SOC="ipq807x_64"
else ifeq ($(CONFIG_TARGET_BOARD), "ipq60xx")
    SOC="ipq60xx_64"
else
    SOC=$(CONFIG_TARGET_BOARD)
endif

ifndef CONFIG_NSS_DRV_BRIDGE_ENABLE
   DRV_MAKE_OPTS += NSS_DRV_BRIDGE_ENABLE=n
endif
ifndef CONFIG_NSS_DRV_C2C_ENABLE
   DRV_MAKE_OPTS += NSS_DRV_C2C_ENABLE=n
endif
ifndef CONFIG_NSS_DRV_CAPWAP_ENABLE
   DRV_MAKE_OPTS += NSS_DRV_CAPWAP_ENABLE=n
endif
ifndef CONFIG_NSS_DRV_CLMAP_ENABLE
   DRV_MAKE_OPTS += NSS_DRV_CLMAP_ENABLE=n
endif
ifndef CONFIG_NSS_DRV_CRYPTO_ENABLE
   DRV_MAKE_OPTS += NSS_DRV_CRYPTO_ENABLE=n
endif
ifndef CONFIG_NSS_DRV_DMA_ENABLE
   DRV_MAKE_OPTS += NSS_DRV_DMA_ENABLE=n
endif
ifndef CONFIG_NSS_DRV_EDMA_ENABLE
   DRV_MAKE_OPTS += NSS_DRV_EDMA_ENABLE=n
endif
ifndef CONFIG_NSS_DRV_GRE_ENABLE
   DRV_MAKE_OPTS += NSS_DRV_GRE_ENABLE=n
endif
ifndef CONFIG_NSS_DRV_GRE_REDIR_ENABLE
   DRV_MAKE_OPTS += NSS_DRV_GRE_REDIR_ENABLE=n
endif
ifndef CONFIG_NSS_DRV_GRE_TUNNEL_ENABLE
   DRV_MAKE_OPTS += NSS_DRV_GRE_TUNNEL_ENABLE=n
endif
ifndef CONFIG_NSS_DRV_IGS_ENABLE
   DRV_MAKE_OPTS += NSS_DRV_IGS_ENABLE=n
endif
ifndef CONFIG_NSS_DRV_IPV4_REASM_ENABLE
   DRV_MAKE_OPTS += NSS_DRV_IPV4_REASM_ENABLE=n
endif
ifndef CONFIG_NSS_DRV_IPV6_ENABLE
   DRV_MAKE_OPTS += NSS_DRV_IPV6_ENABLE=n
endif
ifndef CONFIG_NSS_DRV_IPV6_REASM_ENABLE
   DRV_MAKE_OPTS += NSS_DRV_IPV6_REASM_ENABLE=n
endif
ifndef CONFIG_NSS_DRV_L2TP_ENABLE
   DRV_MAKE_OPTS += NSS_DRV_L2TP_ENABLE=n
endif
ifndef CONFIG_NSS_DRV_LAG_ENABLE
   DRV_MAKE_OPTS += NSS_DRV_LAG_ENABLE=n
endif
ifndef CONFIG_NSS_DRV_MAPT_ENABLE
   DRV_MAKE_OPTS += NSS_DRV_MAPT_ENABLE=n
endif
ifndef CONFIG_NSS_DRV_MATCH_ENABLE
   DRV_MAKE_OPTS += NSS_DRV_MATCH_ENABLE=n
endif
ifndef CONFIG_NSS_DRV_MIRROR_ENABLE
   DRV_MAKE_OPTS += NSS_DRV_MIRROR_ENABLE=n
endif
ifndef CONFIG_NSS_DRV_OAM_ENABLE
   DRV_MAKE_OPTS += NSS_DRV_OAM_ENABLE=n
endif
ifndef CONFIG_NSS_DRV_PORTID_ENABLE
   DRV_MAKE_OPTS += NSS_DRV_PORTID_ENABLE=n
endif
ifndef CONFIG_NSS_DRV_PPE_ENABLE
   DRV_MAKE_OPTS += NSS_DRV_PPE_ENABLE=n
endif
ifndef CONFIG_NSS_DRV_PPPOE_ENABLE
   DRV_MAKE_OPTS += NSS_DRV_PPPOE_ENABLE=n
endif
ifndef CONFIG_NSS_DRV_PPTP_ENABLE
   DRV_MAKE_OPTS += NSS_DRV_PPTP_ENABLE=n
endif
ifndef CONFIG_NSS_DRV_PVXLAN_ENABLE
   DRV_MAKE_OPTS += NSS_DRV_PVXLAN_ENABLE=n
endif
ifndef CONFIG_NSS_DRV_QRFS_ENABLE
   DRV_MAKE_OPTS += NSS_DRV_QRFS_ENABLE=n
endif
ifndef CONFIG_NSS_DRV_QVPN_ENABLE
   DRV_MAKE_OPTS += NSS_DRV_QVPN_ENABLE=n
endif
ifndef CONFIG_NSS_DRV_RMNET_ENABLE
   DRV_MAKE_OPTS += NSS_DRV_RMNET_ENABLE=n
endif
ifndef CONFIG_NSS_DRV_SHAPER_ENABLE
   DRV_MAKE_OPTS += NSS_DRV_SHAPER_ENABLE=n
endif
ifndef CONFIG_NSS_DRV_SJACK_ENABLE
   DRV_MAKE_OPTS += NSS_DRV_SJACK_ENABLE=n
endif
ifndef CONFIG_NSS_DRV_TRUSTSEC_ENABLE
   DRV_MAKE_OPTS += NSS_DRV_TRUSTSEC_ENABLE=n
endif
ifndef CONFIG_NSS_DRV_TSTAMP_ENABLE
   DRV_MAKE_OPTS += NSS_DRV_TSTAMP_ENABLE=n
endif
ifndef CONFIG_NSS_DRV_TUN6RD_ENABLE
   DRV_MAKE_OPTS += NSS_DRV_TUN6RD_ENABLE=n
endif
ifndef CONFIG_NSS_DRV_TUNIPIP6_ENABLE
   DRV_MAKE_OPTS += NSS_DRV_TUNIPIP6_ENABLE=n
endif
ifndef CONFIG_NSS_DRV_VIRT_IF_ENABLE
   DRV_MAKE_OPTS += NSS_DRV_VIRT_IF_ENABLE=n
endif
ifndef CONFIG_NSS_DRV_VLAN_ENABLE
   DRV_MAKE_OPTS += NSS_DRV_VLAN_ENABLE=n
endif
ifndef CONFIG_NSS_DRV_VXLAN_ENABLE
   DRV_MAKE_OPTS += NSS_DRV_VXLAN_ENABLE=n
endif
ifndef CONFIG_NSS_DRV_WIFI_ENABLE
   DRV_MAKE_OPTS += NSS_DRV_WIFI_ENABLE=n
endif

ifeq ($(CONFIG_TARGET_BOARD), "ipq806x")
    TARGET_NSS_MINOR_VERSION=2
    DRV_MAKE_OPTS+= TARGET_NSS_MINOR_VERSION=$(TARGET_NSS_MINOR_VERSION)
endif

define Build/Configure
	$(CP) $(TOPDIR)/$(SOURCE)/files/nss-firmware/nss_fw_version.h $(PKG_BUILD_DIR)/exports/nss_fw_version.h
	sed -i "s/define NSS_FW_VERSION_MAJOR.*/define NSS_FW_VERSION_MAJOR 11/" $(PKG_BUILD_DIR)/exports/nss_fw_version.h
	sed -i "s/define NSS_FW_VERSION_MINOR.*/define NSS_FW_VERSION_MINOR $(TARGET_NSS_MINOR_VERSION)/" $(PKG_BUILD_DIR)/exports/nss_fw_version.h
	$(LN) arch/nss_$(SOC).h $(PKG_BUILD_DIR)/exports/nss_arch.h
endef

define Build/Compile
	$(KERNEL_MAKE) $(strip $(DRV_MAKE_OPTS)) \
		M="$(PKG_BUILD_DIR)" \
		EXTRA_CFLAGS="$(EXTRA_CFLAGS)" SoC="$(subtarget)" \
		modules
endef

$(eval $(call KernelPackage,qca-nss-drv))
