#
# This software is licensed under the Public Domain.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=influxdb
PKG_VERSION:=1.6.0rc1
PKG_RELEASE:=1

PKG_SOURCE:=v$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://github.com/influxdata/influxdb/archive/
PKG_HASH:=687da107b27aed78f609fd637397d896bde3176ef1b7f1c911fb54d980e31fe2

PKG_MAINTAINER:=Manfred Gschweidl <m.gschweidl@gmail.com>
PKG_LICENSE_FILES:=LICENSE

#PKG_INSTALL:=1
#PKG_BUILD_DEPENDS:=protobuf btrfs-progs

# Default:
# PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)
# DUMP
#DUMP:=1
PKG_USE_GO:=1
PKG_GO_IMPORT_PATH:=github.com/influxdata/influxdb

include $(INCLUDE_DIR)/package.mk

#go_modules:=github.com/google/gopacket github.com/google/gopacket/layers \
#	github.com/google/gopacket/pcap github.com/google/gopacket/pfring \
#	github.com/google/gopacket/afpacket github.com/gopacket/tcpassembly

#go_modules:=github.com/google/gopacket github.com/google/gopacket/layers \
#	github.com/google/gopacket/pcap

#TAR_CMD:=$(HOST_TAR) -C $(PKG_BUILD_DIR) --strip-components 1 $(TAR_OPTIONS)

define Package/influxdb
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

define Package/influxdb/description
 InfluxDB is an open source time series database with no external dependencies.
 It's useful for recording metrics, events, and performing analytics.
endef

#define Build/Prepare
#	$(call Build/GO/Prepare/Default)
#endef

define Build/Compile
	$(call copy_go_source)
	(cd $(GO_PKG_COMPILE_PATH) && dep ensure)
	(cd $(GO_PKG_COMPILE_PATH) && go clean ./...)
	(cd $(GO_PKG_COMPILE_PATH) && patch -p1 < vendor_patches/010-boldtdb_mips_mipsel.patch)
	(cd $(GO_PKG_COMPILE_PATH) && go install ./...)
endef

define Build/InstallDev
	$(call Build/GO/InstallDev/Default,$(1))
endef

define Package/influxdb/install
	$(call Package/GO/Install/Default,$(1),influx influx_inspect influx_stress influx_tsm influxd,none,none)
endef

$(eval $(call BuildPackage,influxdb))
