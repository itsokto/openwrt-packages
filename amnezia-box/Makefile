include $(TOPDIR)/rules.mk

PKG_NAME:=sing-box
PKG_VERSION:=1.13.5
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://codeload.github.com/itsokto/amnezia-box/tar.gz/v$(PKG_VERSION)-awg2.0?
PKG_HASH:=c26b813687ee2751f7b14ff8c7699c7a920a9617f93990c2e6f34ef5ac174942

PKG_LICENSE:=GPL-3.0-or-later
PKG_LICENSE_FILES:=LICENSE
PKG_MAINTAINER:=Van Waholtz <brvphoenix@gmail.com>
PKG_CPE_ID:=cpe:/a:sagernet:sing-box

PKG_BUILD_DEPENDS:=golang/host
PKG_BUILD_PARALLEL:=1
PKG_BUILD_FLAGS:=no-mips16
PKG_BUILD_DIR:=$(BUILD_DIR)/amnezia-box-$(PKG_VERSION)-awg2.0

PKG_CHECK_FORMAT_SECURITY:=0
STRIP:=:

GO_PKG:=github.com/sagernet/sing-box
GO_PKG_BUILD_PKG:=$(GO_PKG)/cmd/sing-box
GO_PKG_LDFLAGS_X:=$(GO_PKG)/constant.Version=$(PKG_VERSION)

GO_PKG_TAGS:=with_gvisor,with_quic,with_dhcp,with_wireguard,with_utls,with_acme,with_clash_api,with_awg

include $(INCLUDE_DIR)/package.mk
include $(TOPDIR)/feeds/packages/lang/golang/golang-package.mk

define Package/sing-box
  TITLE:=The universal proxy platform (AmneziaWG Fork)
  SECTION:=net
  CATEGORY:=Network
  URL:=https://github.com/hoaxisr/amnezia-box
  DEPENDS:=$(GO_ARCH_DEPENDS) +ca-bundle +kmod-inet-diag +kmod-tun +kmod-amneziawg
  USERID:=sing-box=5566:sing-box=5566
endef

define Package/sing-box/description
  A custom fork of sing-box with AmneziaWG support, alongside hysteria, SOCKS, 
  Shadowsocks, ShadowTLS, Tor, trojan, VLess, VMess, WireGuard.
endef

define Package/sing-box/conffiles
/etc/config/sing-box
/etc/sing-box/
endef

define Package/sing-box/install
	$(INSTALL_DIR) $(1)/usr/bin/
	$(INSTALL_BIN) $(GO_PKG_BUILD_BIN_DIR)/sing-box $(1)/usr/bin/sing-box

	$(INSTALL_DIR) $(1)/etc/sing-box
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/release/config/config.json $(1)/etc/sing-box/ || true

	$(INSTALL_DIR) $(1)/etc/config/
	$(INSTALL_CONF) ./files/sing-box.conf $(1)/etc/config/sing-box
	
	$(INSTALL_DIR) $(1)/etc/init.d/
	$(INSTALL_BIN) ./files/sing-box.init $(1)/etc/init.d/sing-box
endef

$(eval $(call GoPackage,sing-box))
$(eval $(call BuildPackage,sing-box))