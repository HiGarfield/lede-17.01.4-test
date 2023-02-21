include $(TOPDIR)/rules.mk

PKG_NAME:=wifi-csi
PKG_VERSION:=1
PKG_RELEASE:=1

include $(INCLUDE_DIR)/package.mk

define Package/wifi-csi
  SECTION:=net
  CATEGORY:=Network
  TITLE:=Wifi CSI tool
  DEPENDS:=@TARGET_ar71xx
endef

define Package/wifi-csi/description
  Wifi CSI tool.
endef

define Package/wifi-csi/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/recv_csi $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/send_data $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/send_Data_con $(1)/usr/bin/
endef

$(eval $(call BuildPackage,wifi-csi))
