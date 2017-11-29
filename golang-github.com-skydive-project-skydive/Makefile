#
# This software is licensed under the Public Domain.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=golang-github.com-skydive-project-skydive
PKG_VERSION:=2017-11-29
PKG_RELEASE:=1
PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=http://lxd-phabricator03.lxd.mammoth.home/source/skydive_local.git
PKG_SOURCE:=$(PKG_NAME).tar.gz
PKG_SOURCE_VERSION:=HEAD
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-temp
PKG_HASH:=f8c00bd306db450c32934a08a775ebc1411bb0bc6f395650c183b48ea8fa142e

PKG_MAINTAINER:=Manfred Gschweidl <m.gschweidl@gmail.com>
PKG_LICENSE_FILES:=LICENSE

PKG_INSTALL:=1
PKG_BUILD_DEPENDS:=libprotobuf-c protobuf

# Default:
# PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)
# DUMP
#DUMP:=1
PKG_USE_GO:=1
PKG_GO_IMPORT_PATH:=github.com/skydive-project/skydive
#export PKG_USE_GO_BUILD_PATH:=1

include $(INCLUDE_DIR)/package.mk

#go_modules:=github.com/google/gopacket github.com/google/gopacket/layers \
#	github.com/google/gopacket/pcap github.com/google/gopacket/pfring \
#	github.com/google/gopacket/afpacket github.com/gopacket/tcpassembly

go_modules:=github.com/google/gopacket github.com/google/gopacket/layers \
	github.com/google/gopacket/pcap

PKG_BUILD_DIR_ORIG:=$(PKG_BUILD_DIR)
TAR_CMD:=$(HOST_TAR) -C $(PKG_BUILD_DIR_ORIG)/src/$(PKG_GO_IMPORT_PATH) --strip-components 1 $(TAR_OPTIONS)

PKG_BUILD_DIR:=$(PKG_BUILD_DIR_ORIG)/src/$(PKG_GO_IMPORT_PATH)
export GOPATH:=$(PKG_BUILD_DIR_ORIG)

define Package/golang-github.com-skydive-project-skydive
	SUBMENU:=Golang
	SECTION:=lang
	CATEGORY:=Languages
	TITLE:=Golang Skydive network analyzer V$(PKG_VERSION) (local)
	URL:=http://lxd-phabricator03.lxd.mammoth.home/source/skydive_local.git
	MAINTAINER:=Manfred Gschweidl <m.gschweidl@gmail.com>
	DEPENDS:=+libpcap +libxml2 +golang-github.com-google-gopacket
	#DEFAULT:=n
endef

define Package/golang-github.com-skydive-project-skydive/description
	Network analyzer
endef

#define Build/Prepare
#	$(call Build/GO/Prepare/Default)
#endef

define Build/Compile
	(cd $(PKG_BUILD_DIR) && make install)
endef

define Build/InstallDev
#	$(call Build/GO/InstallDev/Default,$(1))
endef

define Package/golang-github.com-skydive-skydive-skydive/install
	$(call Package/GO/Install/Default,$(1))
endef

$(eval $(call BuildPackage,golang-github.com-skydive-project-skydive))

