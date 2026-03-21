include $(TOPDIR)/rules.mk

PKG_NAME:=clash-core
PKG_VERSION:=1.18
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://github.com/simonchen/clash-core/archive/refs/tags
PKG_HASH:=bfa197c1046b247e8a73d4641d9787ffe1c3ecf306891785a5eabb2aa0d220cc

PKG_MAINTAINER:=simonchen
PKG_LICENSE:=MPL-2.0
PKG_LICENSE_FILES:=LICENSE

PKG_BUILD_DIR:=$(BUILD_DIR)/clash-core-$(PKG_VERSION)
PKG_BUILD_DEPENDS:=golang/host upx/host
PKG_BUILD_PARALLEL:=1
PKG_USE_MIPS16:=0
PKG_BUILD_FLAGS:=no-mips16

GO_PKG:=github.com/simonchen/clash-core
GO_PKG_LDFLAGS:=-s -w
#GO_PKG_BUILD_PKG:=$(GO_PKG)/test
GO_PKG_LDFLAGS_X:=$(GO_PKG)/constant.Version=v$(PKG_VERSION)
BuildTime:=$(shell date -u "+%Y-%m-%d_%H:%M:%S/%Z")
GO_PKG_LDFLAGS_X+=$(GO_PKG)/constant.BuildTime="$(BuildTime)"

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/../feeds/packages/lang/golang/golang-package.mk

define Package/clash-core
  TITLE:=A rule-based tunnel in Go
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=Web Servers/Proxies
  URL:=https://github.com/simonchen/clash-core
  DEPENDS:=$(GO_ARCH_DEPENDS) \
		+iptables \
		+iptables-mod-tproxy \
		+libuci-lua \
		+lyaml \
		+ca-bundle
endef

define Package/clash-core/description
	Clash Core, A rule based tunnel in Go, support VMess, Shadowsocks,
	Trojan, Snell protocol for remote connections.
endef

define Package/clash-core/install
	$(call GoPackage/Package/Install/Bin,$(PKG_INSTALL_DIR))
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_DIR) $(1)/etc/openclash/core
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/clash-core $(1)/usr/bin/clash
	# Compress the binary with UPX
	$(STAGING_DIR_HOST)/bin/upx --lzma --best $(1)/usr/bin/clash
	ln -sf /usr/bin/clash $(1)/etc/openclash/core/clash
endef

$(eval $(call BuildPackage,clash-core))
