#
# This software is licensed under the Public Domain.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=traefik
PKG_VERSION:=1.6.2
PKG_RELEASE:=1

PKG_SOURCE_PROTO=git
PKG_SOURCE_URL:=https://github.com/containous/traefik.git
PKG_SOURCE_VERSION:=v$(PKG_VERSION)
PKG_SOURCE_DATE:=2018-05-30
PKG_MIRROR_HASH:=9b46e5eda2dde26baedf3b61aefadf6e293027502d350b52f48959bea26819ed

PKG_MAINTAINER:=Manfred Gschweidl <m.gschweidl@gmail.com>
PKG_LICENSE_FILES:=LICENSE

#PKG_INSTALL:=1
#PKG_BUILD_DEPENDS:=protobuf btrfs-progs

# Default:
# PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)
# DUMP
#DUMP:=1
PKG_USE_GO:=1
PKG_GO_IMPORT_PATH:=github.com/containous/traefik

include $(INCLUDE_DIR)/package.mk

#go_modules:=github.com/google/gopacket github.com/google/gopacket/layers \
#	github.com/google/gopacket/pcap github.com/google/gopacket/pfring \
#	github.com/google/gopacket/afpacket github.com/gopacket/tcpassembly

#go_modules:=github.com/google/gopacket github.com/google/gopacket/layers \
#	github.com/google/gopacket/pcap

#TAR_CMD:=$(HOST_TAR) -C $(PKG_BUILD_DIR) --strip-components 1 $(TAR_OPTIONS)

define Package/traefik
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

define Package/traefik/description
 InfluxDB is an open source time series database with no external dependencies.
 It's useful for recording metrics, events, and performing analytics.
endef

#define Build/Prepare
#	$(call Build/GO/Prepare/Default)
#endef

define Build/Compile
	$(call copy_go_source)
	(cd $(GO_PKG_COMPILE_PATH) && go get -v github.com/containous/go-bindata/...)
	(cd $(GO_PKG_COMPILE_PATH) && go generate -v)
	(cd $(GO_PKG_COMPILE_PATH) && go build -v ./cmd/traefik)
	$(CP) $(GO_PKG_COMPILE_PATH)/traefik $(GO_PKG_GOPATH)/$(GO_PKG_BIN_PATH)/
endef

#define Build/InstallDev
#	$(call Build/GO/InstallDev/Default,$(1))
#endef

define Package/traefik/install
	$(call Package/GO/Install/Default,$(1),traefik,none,none)
endef

$(eval $(call BuildPackage,traefik))
