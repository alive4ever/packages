#
# Copyright (C) 2018 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=perl-file-slurp
PKG_VERSION:=9999.19
PKG_RELEASE:=1

PKG_SOURCE_URL:=https://www.cpan.org/authors/id/U/UR/URI
PKG_SOURCE:=File-Slurp-$(PKG_VERSION).tar.gz
PKG_HASH:=ce29ebe995097ebd6e9bc03284714cdfa0c46dc94f6b14a56980747ea3253643

PKG_LICENSE:=GPL-1.0+ Artistic-1.0-Perl
PKG_MAINTAINER:=Marcel Denia <naoir@gmx.net>

PKG_BUILD_DIR:=$(BUILD_DIR)/perl/File-Slurp-$(PKG_VERSION)
HOST_BUILD_DIR:=$(BUILD_DIR_HOST)/perl/File-Slurp-$(PKG_VERSION)
HOST_BUILD_DEPENDS:=perl/host

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/host-build.mk
include ../perl/perlmod.mk

define Package/perl-file-slurp
  SUBMENU:=Perl
  SECTION:=lang
  CATEGORY:=Languages
  TITLE:=Read/write/append files quickly
  URL:=http://search.cpan.org/dist/File-Listing/
  DEPENDS:=perl +perlbase-essential
endef

define Package/perl-file-slurp/definition
  File::Slurp provides some quickie routines that read from, append to or
  overwrite a file - with a single subroutine call. Their main intention is to
  save a couple of lines of code over and over again, but they do not do anything
  fancy. 
endef

define Host/Configure
        $(call perlmod/host/Configure,,,)
endef

define Host/Compile
        $(call perlmod/host/Compile,,)
endef

define Host/Install
        $(call perlmod/host/Install,$(1),)
endef

define Build/Configure
        $(call perlmod/Configure,,)
endef

define Build/Compile
        $(call perlmod/Compile,,)
endef

define Package/perl-file-slurp/install
        $(call perlmod/Install,$(1),File/Slurp auto/File/Slurp)
endef

$(eval $(call BuildPackage,perl-file-slurp))
$(eval $(call HostBuild))
