include $(TOPDIR)/rules.mk

PKG_NAME:=qca-nss-macsec
PKG_RELEASE:=2
PKG_SOURCE_URL:=https://git.codelinaro.org/clo/qsdk/oss/lklm/nss-macsec
PKG_SOURCE_PROTO:=git
PKG_SOURCE_DATE:=2023-10-16
PKG_SOURCE_VERSION:=b94e22d
PKG_MIRROR_HASH:=6066b1d4b9f21e96fd151be0b12e5461472d55f9b701a59596418d39fb60b812
QSDK_VERSION:=12.4.5
PKG_VERSION:=$(QSDK_VERSION).$(subst -,.,$(PKG_SOURCE_DATE))~$(PKG_SOURCE_VERSION)

PKG_BUILD_PARALLEL:=1
PKG_FLAGS:=nonshared

include $(INCLUDE_DIR)/kernel.mk
include $(INCLUDE_DIR)/package.mk

define KernelPackage/qca-nss-macsec
  SECTION:=kernel
  CATEGORY:=Kernel modules
  SUBMENU:=Network Devices
  DEPENDS:=@(TARGET_qualcommax_ipq807x||TARGET_ipq60xx) \
  				 +libc \
  				 @BROKEN
  TITLE:=Kernel driver for NSS macsec
  FILES:=$(PKG_BUILD_DIR)/qca-nss-macsec.ko
  AUTOLOAD:=$(call AutoLoad,52,qca-nss-macsec)
endef

define KernelPackage/qca-nss-macsec/Description
This package contains a MACSEC driver for QCA chipset
endef

EXTRA_CFLAGS+= \
	-Wno-missing-prototypes \
	-Wno-missing-declarations

QCA_NSS_MACSEC_CONFIG_OPTS+= TOOL_PATH=$(TOOLCHAIN_DIR)/bin/ \
                SYS_PATH=$(LINUX_DIR) \
                TOOLPREFIX=$(TARGET_CROSS) \
                KVER=$(LINUX_VERSION) \
                CFLAGS="$(TARGET_CFLAGS) $(EXTRA_CFLAGS)" \
                LDFLAGS="$(TARGET_LDFLAGS)" \
                ARCH=$(LINUX_KARCH)

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include/qca-nss-macsec
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_BUILD_DIR)/include/nss_macsec_types.h $(1)/usr/include/qca-nss-macsec
	$(CP) $(PKG_BUILD_DIR)/include/nss_macsec_secy.h $(1)/usr/include/qca-nss-macsec
	$(CP) $(PKG_BUILD_DIR)/include/nss_macsec_secy_rx.h $(1)/usr/include/qca-nss-macsec
	$(CP) $(PKG_BUILD_DIR)/include/nss_macsec_secy_tx.h $(1)/usr/include/qca-nss-macsec
	$(CP) $(PKG_BUILD_DIR)/libfal.so $(1)/usr/lib
endef

ifeq ($(CONFIG_TARGET_SUBTARGET), "ipq807x")
	SOC=ipq807x_64
	subtarget:=$(CONFIG_TARGET_SUBTARGET)
else ifeq ($(CONFIG_TARGET_SUBTARGET), "ipq60xx")
	SOC=ipq60xx_64
	subtarget:=$(CONFIG_TARGET_SUBTARGET)
else ifeq ($(CONFIG_TARGET_SUBTARGET), "ipq50xx")
	SOC=ipq50xx_64
	subtarget:=$(CONFIG_TARGET_SUBTARGET)
endif

define Build/Compile
	+$(KERNEL_MAKE) \
		M="$(PKG_BUILD_DIR)" \
		SoC="$(subtarget)" \
		EXTRA_CFLAGS="$(EXTRA_CFLAGS)" \
		$(PKG_JOBS) \
		modules
	+$(MAKE) -C $(PKG_BUILD_DIR) $(strip $(QCA_NSS_MACSEC_CONFIG_OPTS)) $(PKG_JOBS) -f Makefile.shell
endef

define KernelPackage/qca-nss-macsec/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_DIR) $(1)/usr/lib
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/*.so $(1)/usr/lib/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/macsec_shell $(1)/usr/sbin/
endef

$(eval $(call KernelPackage,qca-nss-macsec))
