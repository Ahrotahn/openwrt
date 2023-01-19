include $(TOPDIR)/rules.mk

PKG_NAME:=nxp-pcie9098
PKG_VERSION=230128
PKG_RELEASE=1

PKG_LICENSE:=GPLv2
PKG_LICENSE_FILES:=

PKG_SOURCE_URL:=https://github.com/Ahrotahn/openwrt-nxp-pcie9098
PKG_MIRROR_HASH:=4d3627546d22a73f78003458ef16ae63b2644348f4e742ba4643a5f6134621c9
PKG_SOURCE_PROTO:=git
PKG_SOURCE_VERSION:=bb3a5198a6c51b99b6d71ffff9996656318b23eb

PKG_BUILD_PARALLEL:=1
PKG_FLAGS:=nonshared

include $(INCLUDE_DIR)/kernel.mk
include $(INCLUDE_DIR)/package.mk

define KernelPackage/nxp-pcie9098
	SUBMENU:=Wireless Drivers
	TITLE:=NXP pcie 9098 driver
	DEPENDS:=+kmod-mac80211 +kmod-cfg80211 +kmod-mwifiex-pcie \
		+@DRIVER_11AC_SUPPORT +@DRIVER_11AX_SUPPORT \
		@PCI_SUPPORT @TARGET_mvebu
	FILES:=\
		$(PKG_BUILD_DIR)/driver/mlan.ko \
		$(PKG_BUILD_DIR)/driver/moal.ko
	AUTOLOAD:=$(call AutoProbe,mlan moal)
	MODPARAMS.moal:=mod_para=nxp/wifi_mod_para.conf drvdbg=0x6
endef

NOSTDINC_FLAGS := \
	$(KERNEL_NOSTDINC_FLAGS) \
	-I$(PKG_BUILD_DIR) \
	-I$(STAGING_DIR)/usr/include/mac80211-backport/uapi \
	-I$(STAGING_DIR)/usr/include/mac80211-backport \
	-I$(STAGING_DIR)/usr/include/mac80211/uapi \
	-I$(STAGING_DIR)/usr/include/mac80211 \
	-include backport/backport.h

define Build/Compile
	+$(MAKE) $(PKG_JOBS) -C "$(LINUX_DIR)" \
		$(KERNEL_MAKE_FLAGS) \
		M="$(PKG_BUILD_DIR)/driver" \
		NOSTDINC_FLAGS="$(NOSTDINC_FLAGS)" \
		modules
endef

define KernelPackage/nxp-pcie9098/install
	$(INSTALL_DIR) $(1)/lib/firmware/nxp
	$(INSTALL_DATA) \
		$(PKG_BUILD_DIR)/firmware/nxp/FwImage_9098_PCIE/pcieuart9098_combo_v1.bin \
		$(1)/lib/firmware/nxp/pcieuart9098_combo_v1.bin
	$(INSTALL_DATA) \
		$(PKG_BUILD_DIR)/wifi_mod_para.conf \
		$(1)/lib/firmware/nxp/wifi_mod_para.conf
endef

$(eval $(call KernelPackage,nxp-pcie9098))
