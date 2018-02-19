include $(TOPDIR)/rules.mk

PKG_NAME:=sbsigntool
PKG_VERSION:=0.9.1
PKG_LICENSE:=GPL-3.0+ OpenSSL
PKG_MAINTAINER:=Alif M. Ahmad <alive4ever@live.com>

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://git.kernel.org/pub/scm/linux/kernel/git/jejb/sbsigntools.git
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_VERSION:=79616f66875f619788502fbf7ddc9cc1a0b58187
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION)-$(PKG_SOURCE_VERSION).tar.gz

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
