#
# Copyright (C) 2006-2015 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=libpng
PKG_VERSION:=1.6.48
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.xz
PKG_SOURCE_URL:=@SF/libpng
PKG_HASH:=46fd06ff37db1db64c0dc288d78a3f5efd23ad9ac41561193f983e20937ece03

PKG_MAINTAINER:=Jo-Philipp Wich <jo@mein.io>
PKG_LICENSE:=Libpng GPL-2.0-or-later BSD-3-Clause
PKG_LICENSE_FILES:=LICENSE contrib/gregbook/COPYING contrib/gregbook/LICENSE
PKG_CPE_ID:=cpe:/a:libpng:libpng

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/cmake.mk

define Package/libpng
  SECTION:=libs
  CATEGORY:=Libraries
  DEPENDS:=+zlib
  TITLE:=A PNG format files handling library
  URL:=http://www.libpng.org/pub/png/libpng.html
endef

CMAKE_OPTIONS += \
	-DCMAKE_POSITION_INDEPENDENT_CODE=ON \
	-DPNG_TOOLS=OFF \
	-DPNG_TESTS=OFF \
	-DPNG_FRAMEWORK=OFF \
	-DPNG_HARDWARE_OPTIMIZATIONS=ON \
	-DPNG_ARM_NEON=$(if $(or $(findstring aarch64,$(CONFIG_ARCH)),$(findstring neon,$(CONFIG_CPU_TYPE))),on,off) \
	-DPNG_LOONGARCH_LSX=off \
	-DPNG_MIPS_MMI=off \
	-DPNG_MIPS_MSA=off \
	-DPNG_POWERPC_VSX=off \
	-Dld-version-script=OFF

define Build/InstallDev
	$(call Build/InstallDev/cmake,$(1))
	$(SED) 's,^\(prefix\|exec_prefix\)=.*,\1=$(STAGING_DIR)/usr,g' $(1)/usr/bin/libpng{,16}-config
	$(SED) '/^includedir=/s|/usr|$$$${prefix}|' $(1)/usr/bin/libpng{,16}-config
	$(SED) '/^libdir=/s|/usr|$$$${prefix}|' $(1)/usr/bin/libpng{,16}-config
	$(SED) 's,/usr/include,$$$${prefix}/include,g' $(1)/usr/lib/pkgconfig/libpng16.pc
	$(SED) 's,/usr/lib,$$$${exec_prefix}/lib,g' $(1)/usr/lib/pkgconfig/libpng16.pc
	$(INSTALL_DIR) $(2)/bin
	for f in libpng{,16}-config; do \
		$(LN) ../../usr/bin/$$$$f $(2)/bin/ ; \
	done
endef

define Package/libpng/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libpng16.so* $(1)/usr/lib/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libpng.so $(1)/usr/lib/
endef

$(eval $(call BuildPackage,libpng))
