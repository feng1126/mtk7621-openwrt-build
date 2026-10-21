include $(TOPDIR)/rules.mk

PKG_NAME:=default-settings
PKG_VERSION:=1
PKG_RELEASE:=1
PKG_LICENSE:=GPL-2.0-only

include $(INCLUDE_DIR)/package.mk

define Package/default-settings
  SECTION:=luci
  CATEGORY:=LuCI
  TITLE:=Newifi D2 interface defaults
  DEPENDS:=+luci +luci-theme-argon
  PKGARCH:=all
endef

define Build/Compile
endef

define Package/default-settings/install
	$(INSTALL_DIR) $(1)/etc/uci-defaults
	$(INSTALL_BIN) ./files/99-newifi-d2-ui $(1)/etc/uci-defaults/
endef

$(eval $(call BuildPackage,default-settings))
