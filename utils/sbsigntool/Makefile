include $(TOPDIR)/rules.mk

PKG_NAME:=sbsigntool
PKG_VERSION:=0.6
PKG_HASH:=84fb0c8f6fb1e79aa418a4f70a3139b38d5630043b28291c875f383e9b4294b8
PKG_LICENSE:=GPL-3.0+ OpenSSL
PKG_MAINTAINER:=Alif M. Ahmad <alive4ever@live.com>

PKG_SOURCE:=$(PKG_NAME)_$(PKG_VERSION).orig.tar.gz
PKG_SOURCE_URL:=http://http.debian.net/debian/pool/main/s/$(PKG_NAME)

PKG_BUILD_DEPENDS:=gnu-efi binutils
PKG_FIXUP:=autoreconf
PKG_INSTALL:=1

HOST_BUILD_DEPENDS:=gnu-efi/host

include $(INCLUDE_DIR)/host-build.mk
include $(INCLUDE_DIR)/package.mk

define Package/sbsigntool
  SECTION:=utils
  CATEGORY:=Utilities
  DEPENDS:= +libuuid +libopenssl
  DEPENDS+= @(x86_64||i386)
  TITLE:=Tools to manipulate signatures on UEFI binaries and drivers
endef

define Package/sbsigntool/description
  This package installs tools which can cryptographically sign EFI binaries
  and drivers.
endef

define Build/Prepare
$(call Build/Prepare/Default)
	sed -i 's#EFI_ARCH=$$$$(uname -m)#EFI_ARCH=$(ARCH)#' \
	$(PKG_BUILD_DIR)/configure.ac
	sed -i "s#-I/usr/include/efi#-I$(STAGING_DIR)/usr/include/efi#g" \
	$(PKG_BUILD_DIR)/configure.ac
endef

define Package/sbsigntool/install
$(call Build/Install/Default,install-exec)
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/* $(1)/usr/bin
endef

define Host/Prepare
$(call Host/Prepare/Default)
	sed -i "s#-I/usr/include/efi#-I$(STAGING_DIR_HOST)/include/efi#g" \
	$(HOST_BUILD_DIR)/configure.ac
endef

define Host/Install
	$(INSTALL_DIR) $(STAGING_DIR_HOST)/bin
	$(INSTALL_BIN) $(HOST_BUILD_DIR)/src/{sbattach,sbkeysync,sbsiglist,sbsign,sbvarsign,sbverify} $(STAGING_DIR_HOST)/bin
endef

$(eval $(call HostBuild,sbsigntool))
$(eval $(call BuildPackage,sbsigntool))
