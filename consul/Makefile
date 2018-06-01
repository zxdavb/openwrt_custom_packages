#
# This software is licensed under the Public Domain.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=consul
PKG_VERSION:=1.1.0
PKG_RELEASE:=1

PKG_SOURCE_PROTO=git
PKG_SOURCE_URL:=https://github.com/hashicorp/consul.git
PKG_SOURCE_VERSION:=v$(PKG_VERSION)
PKG_SOURCE_DATE:=2018-05-31
PKG_MIRROR_HASH:=6f30726164fa04b3cf7df2872a69cf6509e9cade5f03ae07ac7027c0bd7fd5de

PKG_MAINTAINER:=Manfred Gschweidl <m.gschweidl@gmail.com>
PKG_LICENSE_FILES:=LICENSE

#PKG_INSTALL:=1
#PKG_BUILD_DEPENDS:=gx/host gx-go/host

# Default:
# PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)
# DUMP
#DUMP:=1
PKG_USE_GO:=1
PKG_GO_IMPORT_PATH:=github.com/hashicorp/consul

include $(INCLUDE_DIR)/package.mk

#go_modules:=github.com/google/gopacket github.com/google/gopacket/layers \
#	github.com/google/gopacket/pcap github.com/google/gopacket/pfring \
#	github.com/google/gopacket/afpacket github.com/gopacket/tcpassembly

#go_modules:=github.com/google/gopacket github.com/google/gopacket/layers \
#	github.com/google/gopacket/pcap

#TAR_CMD:=$(HOST_TAR) -C $(PKG_BUILD_DIR) --strip-components 1 $(TAR_OPTIONS)

define Package/consul
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

define Package/consul/description
 InfluxDB is an open source time series database with no external dependencies.
 It's useful for recording metrics, events, and performing analytics.
endef

#define Build/Prepare
#	$(call Build/GO/Prepare/Default)
#endef

export PATH=$(GOPATH)/bin:$(GOPATH)/$(GO_PKG_BIN_PATH):$(TARGET_PATH_PKG)
define Build/Compile
	$(call copy_go_source)
	( cd $(GO_PKG_COMPILE_PATH) && PATH=$(PATH) CONSUL_DEV=1 make )
endef

#define Build/InstallDev
#	$(call Build/GO/InstallDev/Default,$(1))
#endef

define Build/Install
	mkdir -p $(GOPATH)/$(GO_PKG_BIN_PATH)
	$(CP) $(GOPATH)/bin/consul $(GOPATH)/$(GO_PKG_BIN_PATH)
endef

define Package/consul/install
	$(call Package/GO/Install/Default,$(1),consul,none,none)
endef

$(eval $(call BuildPackage,consul))
