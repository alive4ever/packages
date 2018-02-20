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

PKG_BUILD_DEPENDS:=gnu-efi binutils sbsigntool/host
PKG_FIXUP:=autoreconf
PKG_INSTALL:=1

HOST_BUILD_DEPENDS:=gnu-efi/host

include $(INCLUDE_DIR)/host-build.mk
include $(INCLUDE_DIR)/package.mk

define Download/ccan
  FILE:=ccan.tar.gz
  PROTO:=git
  URL:=git://git.ozlabs.org/~ccan/ccan
  VERSION:=ec1f716142c77fd6b4ca9939bd780150821ba7a2
  SUBDIR:=lib/ccan.new
endef

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
	gzip -dc $(DL_DIR)/ccan.tar.gz | tar -C $(PKG_BUILD_DIR) -xf -
	rm -rf $(PKG_BUILD_DIR)/lib/ccan.git
	(cd $(PKG_BUILD_DIR)/lib && mv ccan.new ccan.git && cd - )
	sed -i -E 's#\$$$$path/crt0-efi#$(STAGING_DIR)/\$$$$path/crt0-efi#' \
	$(PKG_BUILD_DIR)/configure.ac
	sed -i "s#-I/usr/include/efi#-I$(STAGING_DIR)/usr/include/efi#g" \
	$(PKG_BUILD_DIR)/configure.ac
	(cd $(PKG_BUILD_DIR) && \
	sed -i -e 's,sys/unistd.h,unistd.h,g' \
		"$(PKG_BUILD_DIR)"/lib/ccan.git/ccan/opt/usage.c ; \
	sed -i -e 's,--git-only "$$$$module_dir","$$$$module_srcdir",g' \
		"$(PKG_BUILD_DIR)"/lib/ccan.git/tools/create-ccan-tree ; \
	./lib/ccan.git/tools/create-ccan-tree \
                --build-type=automake lib/ccan \
		"talloc read_write_all build_assert array_size endian list" && \
	cd - )
	( cd $(PKG_BUILD_DIR) && NOCONFIGURE=1 ./autogen.sh && cd - )
endef

define Package/sbsigntool/install
$(call Build/Install/Default,install-exec)
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/* $(1)/usr/bin
endef

define Host/Prepare
$(call Host/Prepare/Default)
	gzip -dc $(DL_DIR)/ccan.tar.gz | tar -C $(HOST_BUILD_DIR) -xf -
	rm -rf $(HOST_BUILD_DIR)/lib/ccan.git
	(cd $(HOST_BUILD_DIR)/lib && mv ccan.new ccan.git && cd - )
	sed -i -E 's#\$$$$path/crt0-efi#$(STAGING_DIR)/\$$$$path/crt0-efi#' \
	$(HOST_BUILD_DIR)/configure.ac
	sed -i "s#-I/usr/include/efi#-I$(STAGING_DIR)/usr/include/efi#g" \
	$(HOST_BUILD_DIR)/configure.ac
	(cd $(HOST_BUILD_DIR) && \
	sed -i -e 's,sys/unistd.h,unistd.h,g' \
		"$(HOST_BUILD_DIR)"/lib/ccan.git/ccan/opt/usage.c ; \
	sed -i -e 's,--git-only "$$$$module_dir","$$$$module_srcdir",g' \
		"$(HOST_BUILD_DIR)"/lib/ccan.git/tools/create-ccan-tree ; \
	./lib/ccan.git/tools/create-ccan-tree \
                --build-type=automake lib/ccan \
		"talloc read_write_all build_assert array_size endian list" && \
	cd - )
	( cd $(HOST_BUILD_DIR) && NOCONFIGURE=1 ./autogen.sh && \
	$(CC) $(CFLAGS) -o ./lib/ccan/configurator \
		 ./lib/ccan.git/tools/configurator/configurator.c && \
	./lib/ccan/configurator | tee ./lib/ccan/config.h && cd - )
endef

define Host/Install
	$(INSTALL_DIR) $(STAGING_DIR_HOST)/bin
	$(INSTALL_BIN) $(HOST_BUILD_DIR)/src/{sbattach,sbkeysync,sbsiglist,sbsign,sbvarsign,sbverify} $(STAGING_DIR_HOST)/bin
endef

$(eval $(call Download,ccan))
$(eval $(call HostBuild,sbsigntool))
$(eval $(call BuildPackage,sbsigntool))
