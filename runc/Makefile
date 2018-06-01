#
# This software is licensed under the Public Domain.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=runc
PKG_VERSION:=1.0.0-containerd
PKG_RELEASE:=1
PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/opencontainers/runc.git
PKG_SOURCE:=$(PKG_NAME).tar.gz
#PKG_SOURCE_VERSION:=v1.0.0-rc5
PKG_SOURCE_VERSION:=69663f0bd4b60df09991c08812a60108003fa340
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-temp
#PKG_HASH:=9f19f4b3d2730df9497335f9336e70f96d6298948ad0ec8a751143141a1eb89d
PKG_HASH:=3af9d3c872d777d9bdf2348de04a402c67c712c2255af493aab1c358bd7b0dff

PKG_MAINTAINER:=Manfred Gschweidl <m.gschweidl@gmail.com>
PKG_LICENSE_FILES:=LICENSE

#PKG_INSTALL:=1
PKG_BUILD_DEPENDS:=libseccomp

# Default:
# PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)
# DUMP
#DUMP:=1
PKG_USE_GO:=1
PKG_GO_IMPORT_PATH:=github.com/opencontainers/runc

include $(INCLUDE_DIR)/package.mk

#go_modules:=github.com/google/gopacket github.com/google/gopacket/layers \
#	github.com/google/gopacket/pcap github.com/google/gopacket/pfring \
#	github.com/google/gopacket/afpacket github.com/gopacket/tcpassembly

#go_modules:=github.com/google/gopacket github.com/google/gopacket/layers \
#	github.com/google/gopacket/pcap

TAR_CMD:=$(HOST_TAR) -C $(PKG_BUILD_DIR) --strip-components 1 $(TAR_OPTIONS)

define Package/runc
	SUBMENU:=Golang
	SECTION:=lang
	CATEGORY:=Languages
	TITLE:=v$(PKG_VERSION) - CLI tool for spawning and running containers
	URL:=http://opencontainers.org
	MAINTAINER:=Manfred Gschweidl <m.gschweidl@gmail.com>
	#DEPENDS:=+libpcap +libxml2 +golang-github.com-google-gopacket
	DEPENDS:=
	#DEFAULT:=n
endef

define Package/runc/description
 runc is a CLI tool for spawning and running containers according
to the OCI specification

endef

#define Build/Prepare
#	$(call Build/GO/Prepare/Default)
#endef

define Build/Compile
	$(call copy_go_source)
	(cd $(GO_PKG_COMPILE_PATH) && make && go install)
endef

define Build/InstallDev
	$(call Build/GO/InstallDev/Default,$(1))
endef

define Package/runc/install
	$(call Package/GO/Install/Default,$(1),,none,none)
endef

$(eval $(call BuildPackage,runc))
