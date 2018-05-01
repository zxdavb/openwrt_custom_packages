#
# This software is licensed under the Public Domain.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=golang-github.com-smira-aptly
PKG_VERSION:=2017-11-29
PKG_RELEASE:=1
PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/smira/aptly.git
PKG_SOURCE:=$(PKG_NAME).tar.gz
PKG_SOURCE_VERSION:=v1.1.1
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-temp
PKG_HASH:=a20825499b230ed7ddf7f0a986037da1eea2e4a82e17dfbfd853ef072f233e6f

PKG_MAINTAINER:=Manfred Gschweidl <m.gschweidl@gmail.com>
PKG_LICENSE_FILES:=LICENSE

PKG_INSTALL:=1
PKG_BUILD_DEPENDS:=libprotobuf-c protobuf

# Default:
# PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)
# DUMP
#DUMP:=1
PKG_USE_GO:=1
PKG_GO_IMPORT_PATH:=github.com/smira/aptly

include $(INCLUDE_DIR)/package.mk

#go_modules:=github.com/google/gopacket github.com/google/gopacket/layers \
#	github.com/google/gopacket/pcap github.com/google/gopacket/pfring \
#	github.com/google/gopacket/afpacket github.com/gopacket/tcpassembly

go_modules:=github.com/google/gopacket github.com/google/gopacket/layers \
	github.com/google/gopacket/pcap

TAR_CMD:=$(HOST_TAR) -C $(PKG_BUILD_DIR) --strip-components 1 $(TAR_OPTIONS)

define Package/golang-github.com-smira-aptly
	SUBMENU:=Golang
	SECTION:=lang
	CATEGORY:=Languages
	TITLE:=Atply v$(PKG_VERSION) - Debian Repository Management
	URL:=http://lxd-phabricator03.lxd.mammoth.home/source/skydive_local.git
	MAINTAINER:=Manfred Gschweidl <m.gschweidl@gmail.com>
	#DEPENDS:=+libpcap +libxml2 +golang-github.com-google-gopacket
	DEPENDS:=
	#DEFAULT:=n
endef

define Package/golang-github.com-smira-aptly/description
	Debian Repository Manger in GO
endef

#define Build/Prepare
#	$(call Build/GO/Prepare/Default)
#endef

define Build/Compile
	$(call copy_go_source)
	(cd $(GOPATH)/src/$(PKG_GO_IMPORT_PATH)/ && make install)
endef

define Build/InstallDev
	$(call Build/GO/InstallDev/Default,$(1))
endef

define Package/golang-github.com-smira-aptly/install
	$(call Package/GO/Install/Default,$(1))
endef

$(eval $(call BuildPackage,golang-github.com-smira-aptly))

