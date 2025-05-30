include $(TOPDIR)/rules.mk

PKG_NAME:=libksba
PKG_VERSION:=1.6.7
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.bz2
PKG_SOURCE_URL:=https://gnupg.org/ftp/gcrypt/$(PKG_NAME)
PKG_HASH:=cf72510b8ebb4eb6693eef765749d83677a03c79291a311040a5bfd79baab763

PKG_MAINTAINER:=Daniel Golle <daniel@makrotopia.org>
PKG_LICENSE:=LGPL-3.0-or-later GPL-2.0-or-later
PKG_LICENSE_FILES:=COPYING.GPLv2 COPYING.GPLv3 COPYING.LGPLv3
PKG_CPE_ID:=cpe:/a:gnupg:libksba

PKG_FIXUP:=autoreconf
PKG_INSTALL:=1
PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk

define Package/libksba
  SECTION:=libs
  CATEGORY:=Libraries
  TITLE:=GnuPG X.509 library
  URL:=https://gnupg.org/software/libksba/index.html
  DEPENDS:=+libgpg-error
endef

define Package/libksba/description
Libksba is a library to make the tasks of working with X.509 certificates, CMS data and related objects more easy. 
endef

define Build/InstallDev
	$(INSTALL_DIR) $(2)/bin $(1)/usr/bin
	$(INSTALL_BIN) \
		$(PKG_INSTALL_DIR)/usr/bin/ksba-config \
		$(2)/bin/
	$(SED) \
		's,^\(prefix\|exec_prefix\)=.*,\1=$(STAGING_DIR)/usr,g' \
		$(2)/bin/ksba-config
	ln -sf $(STAGING_DIR)/host/bin/ksba-config $(1)/usr/bin/ksba-config

	$(INSTALL_DIR) $(1)/usr/include
	$(INSTALL_DATA) \
		$(PKG_INSTALL_DIR)/usr/include/ksba.h \
		$(1)/usr/include/

	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) \
		$(PKG_INSTALL_DIR)/usr/lib/libksba.{la,so*} \
		$(1)/usr/lib/

	$(INSTALL_DIR) $(1)/usr/share/aclocal
	$(INSTALL_DATA) \
		$(PKG_INSTALL_DIR)/usr/share/aclocal/ksba.m4 \
		$(1)/usr/share/aclocal/

	$(INSTALL_DIR) $(1)/usr/lib/pkgconfig
	$(INSTALL_DATA) \
		$(PKG_INSTALL_DIR)/usr/lib/pkgconfig/ksba.pc \
		$(1)/usr/lib/pkgconfig
endef

define Package/libksba/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) \
		$(PKG_INSTALL_DIR)/usr/lib/libksba.so.* \
		$(1)/usr/lib/
endef

$(eval $(call BuildPackage,libksba))
