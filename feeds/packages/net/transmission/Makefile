#
# Copyright (C) 2009-2016 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=transmission
PKG_VERSION:=3.00
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.xz
PKG_SOURCE_URL:=@GITHUB/transmission/transmission-releases/master
PKG_HASH:=9144652fe742f7f7dd6657716e378da60b751aaeda8bef8344b3eefc4db255f2

PKG_MAINTAINER:=Rosen Penev <rosenp@gmail.com>
PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(BUILD_VARIANT)/$(PKG_NAME)-$(PKG_VERSION)

PKG_LICENSE:=GPL-2.0+
PKG_LICENSE_FILES:=COPYING

PKG_FIXUP:=autoreconf
PKG_INSTALL:=1
PKG_BUILD_PARALLEL:=1
PKG_USE_MIPS16:=0
PKG_CONFIG_DEPENDS:= \
	CONFIG_LIBCURL_GNUTLS \
	CONFIG_LIBCURL_MBEDTLS \
	CONFIG_LIBCURL_OPENSSL \
	CONFIG_LIBCURL_NOSSL

include $(INCLUDE_DIR)/package.mk

define Package/transmission/template
  SUBMENU:=BitTorrent
  SECTION:=net
  CATEGORY:=Network
  TITLE:=BitTorrent client
  URL:=http://www.transmissionbt.com
  DEPENDS:=+libcurl +libevent2 +libminiupnpc +libnatpmp +libpthread +librt +zlib +LIBCURL_NOSSL:libmbedtls +LIBCURL_GNUTLS:libmbedtls +block-mount
endef

define Package/transmission-daemon
  $(call Package/transmission/template)
  USERID:=transmission=224:transmission=224
endef

define Package/transmission-cli
  $(call Package/transmission/template)
  TITLE+= (utilities)
endef

define Package/transmission-remote
  $(call Package/transmission/template)
  TITLE+= (remote)
endef

define Package/transmission-web
  $(call Package/transmission/template)
  TITLE+= (webinterface)
  DEPENDS:=+transmission-daemon
  PKGARCH:=all
endef


define Package/transmission-daemon/description
 Transmission is a simple BitTorrent client.
 It features a very simple, intuitive interface
 on top on an efficient, cross-platform back-end.
 This package contains the daemon itself.
endef

define Package/transmission-cli/description
 CLI utilities for transmission.
endef

define Package/transmission-remote/description
 CLI remote interface for transmission.
endef

define Package/transmission-web/description
 Webinterface resources for transmission.
endef

define Package/transmission-daemon/conffiles
/etc/config/transmission
endef

TARGET_CFLAGS += -ffunction-sections -fdata-sections -flto
TARGET_LDFLAGS += -Wl,--gc-sections -Wl,--as-needed

CONFIGURE_ARGS += \
	--enable-cli \
	--enable-daemon \
	--enable-external-natpmp \
	--enable-largefile \
	--enable-lightweight \
	--without-gtk \
	--without-kqueue \
	--without-systemd-daemon \
	$(if $(CONFIG_LIBCURL_NOSSL),--with-crypto=polarssl) \
	$(if $(CONFIG_LIBCURL_GNUTLS),--with-crypto=polarssl) \
	$(if $(CONFIG_LIBCURL_MBEDTLS),--with-crypto=polarssl) \
	$(if $(CONFIG_LIBCURL_OPENSSL),--with-crypto=openssl)

define Package/transmission-daemon/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/transmission-daemon $(1)/usr/bin/
	$(INSTALL_DIR) $(1)/etc/init.d/
	$(INSTALL_BIN) files/transmission.init $(1)/etc/init.d/transmission
	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_CONF) files/transmission.config $(1)/etc/config/transmission
	$(INSTALL_DIR) $(1)/etc/uci-defaults
	$(INSTALL_BIN) ./files/99_transmission-settings $(1)/etc/uci-defaults/99_transmission-settings
endef

define Package/transmission-cli/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/transmission-cli \
			$(PKG_INSTALL_DIR)/usr/bin/transmission-create \
			$(PKG_INSTALL_DIR)/usr/bin/transmission-edit \
			$(PKG_INSTALL_DIR)/usr/bin/transmission-show \
			$(1)/usr/bin/
endef

define Package/transmission-remote/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/transmission-remote $(1)/usr/bin/
endef

define Package/transmission-web/install
	$(INSTALL_DIR) $(1)/usr/share/transmission
	$(CP) $(PKG_INSTALL_DIR)/usr/share/transmission/web $(1)/usr/share/transmission/
endef

$(eval $(call BuildPackage,transmission-daemon))
$(eval $(call BuildPackage,transmission-cli))
$(eval $(call BuildPackage,transmission-remote))
$(eval $(call BuildPackage,transmission-web))
