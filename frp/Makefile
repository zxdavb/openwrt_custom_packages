#
# This software is licensed under the Public Domain.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=frp
PKG_VERSION:=0.20.0
PKG_RELEASE:=1

PKG_SOURCE_PROTO=git
PKG_SOURCE_URL:=https://github.com/fatedier/frp.git
PKG_SOURCE_VERSION:=v$(PKG_VERSION)
PKG_SOURCE_DATE:=2018-05-31
PKG_MIRROR_HASH:=783963f45f7ef80d1b954a4f92668b57959e7c1b1baf15e152e0238627edbb71

PKG_MAINTAINER:=Manfred Gschweidl <m.gschweidl@gmail.com>
PKG_LICENSE_FILES:=LICENSE

PKG_USE_GO:=1
PKG_GO_IMPORT_PATH:=github.com/fatedier/frp

include $(INCLUDE_DIR)/package.mk

define Package/frp
	SUBMENU:=Golang
	SECTION:=lang
	CATEGORY:=Languages
	TITLE:=v$(PKG_VERSION) - An open source time series database
	URL:=https://containerd.io
	MAINTAINER:=Manfred Gschweidl <m.gschweidl@gmail.com>
	#DEPENDS:=+libpcap +libxml2 +golang-github.com-google-gopacket
	DEPENDS:=
	#DEFAULT:=n
endef

define Package/frp/description
 InfluxDB is an open source time series database with no external dependencies.
 It's useful for recording metrics, events, and performing analytics.
endef

define Build/Compile
	$(call copy_go_source)
	( cd $(GO_PKG_COMPILE_PATH) && make all )
endef

define Build/Install
	mkdir -p $(GOPATH)/$(GO_PKG_BIN_PATH)
	$(CP) $(GO_PKG_COMPILE_PATH)/bin/{frps,frpc} $(GOPATH)/$(GO_PKG_BIN_PATH)
endef

define Package/frp/install
	$(call Package/GO/Install/Default,$(1),frps frpc,none,none)
endef

$(eval $(call BuildPackage,frp))
