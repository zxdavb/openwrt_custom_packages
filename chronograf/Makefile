#
# This software is licensed under the Public Domain.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=chronograf
PKG_VERSION:=1.5.0.0
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://github.com/influxdata/chronograf/archive/
PKG_HASH:=501f31e65b84804b478e4e0da470f5723dcd54835df4d85408347da13a7ed4b0

PKG_MAINTAINER:=Manfred Gschweidl <m.gschweidl@gmail.com>
PKG_LICENSE_FILES:=LICENSE

PKG_BUILD_DEPENDS:=node/host node-yarn/host go-bindata/host

PKG_USE_GO:=1
PKG_GO_IMPORT_PATH:=github.com/influxdata/chronograf

include $(INCLUDE_DIR)/package.mk

#TAR_CMD:=$(HOST_TAR) -C $(PKG_BUILD_DIR) --strip-components 1 $(TAR_OPTIONS)

define Package/chronograf
	SUBMENU:=Golang
	SECTION:=lang
	CATEGORY:=Languages
	TITLE:=v$(PKG_VERSION) - processing, monitoring and alerting TSD
	URL:=https://influxdata.io
	MAINTAINER:=Manfred Gschweidl <m.gschweidl@gmail.com>
	#DEPENDS:=+libpcap +libxml2 +golang-github.com-google-gopacket
	DEPENDS:=
	#DEFAULT:=n
endef

define Package/chronograf/description
 Open source framework for processing, monitoring, and alerting on time
 series data
endef

define Build/Compile
	$(call copy_go_source)
	(cd $(GO_PKG_COMPILE_PATH) && make)
	(cd $(GO_PKG_COMPILE_PATH) && \
		go install github.com/influxdata/chronograf/cmd/chronograf)
endef

define Build/InstallDev
	$(call Build/GO/InstallDev/Default,$(1))
endef

define Package/chronograf/install
	$(call Package/GO/Install/Default,$(1),chronograf,none,none)
endef

$(eval $(call BuildPackage,chronograf))
