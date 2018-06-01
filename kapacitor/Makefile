#
# This software is licensed under the Public Domain.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=kapacitor
PKG_VERSION:=1.5.0
PKG_RELEASE:=1

PKG_SOURCE:=v$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://github.com/influxdata/kapacitor/archive/
PKG_HASH:=bddb634ee845813b80877935063f92eda35c02c0493b6b4ff2358fadf441c857

PKG_MAINTAINER:=Manfred Gschweidl <m.gschweidl@gmail.com>
PKG_LICENSE_FILES:=LICENSE

PKG_BUILD_DEPENDS:=gdm/host

PKG_USE_GO:=1
PKG_GO_IMPORT_PATH:=github.com/influxdata/kapacitor

include $(INCLUDE_DIR)/package.mk

#TAR_CMD:=$(HOST_TAR) -C $(PKG_BUILD_DIR) --strip-components 1 $(TAR_OPTIONS)

define Package/kapacitor
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

define Package/kapacitor/description
 Open source framework for processing, monitoring, and alerting on time
 series data
endef

define Build/Compile
	$(call copy_go_source)
	(cd $(GO_PKG_COMPILE_PATH) && \
		go get -v github.com/influxdata/kapacitor/cmd/kapacitor && \
		go get -v github.com/influxdata/kapacitor/cmd/kapacitord)
endef

define Build/InstallDev
	$(call Build/GO/InstallDev/Default,$(1))
endef

define Package/kapacitor/install
	$(call Package/GO/Install/Default,$(1),kapacitor kapacitord,none,none)
endef

$(eval $(call BuildPackage,kapacitor))
