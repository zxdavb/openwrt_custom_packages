#
# This software is licensed under the Public Domain.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=containerd
PKG_VERSION:=1.1.0
PKG_RELEASE:=1

PKG_SOURCE:=v$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://github.com/containerd/containerd/archive/
PKG_HASH:=08f057ece7e518b14cce2e9737228a5a899a7b58b78248a03e02f4a6c079eeaf

PKG_MAINTAINER:=Manfred Gschweidl <m.gschweidl@gmail.com>
PKG_LICENSE_FILES:=LICENSE

#PKG_INSTALL:=1
PKG_BUILD_DEPENDS:=protobuf btrfs-progs

# Default:
# PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)
# DUMP
#DUMP:=1
PKG_USE_GO:=1
PKG_GO_IMPORT_PATH:=github.com/containerd/containerd

include $(INCLUDE_DIR)/package.mk

#go_modules:=github.com/google/gopacket github.com/google/gopacket/layers \
#	github.com/google/gopacket/pcap github.com/google/gopacket/pfring \
#	github.com/google/gopacket/afpacket github.com/gopacket/tcpassembly

#go_modules:=github.com/google/gopacket github.com/google/gopacket/layers \
#	github.com/google/gopacket/pcap

#TAR_CMD:=$(HOST_TAR) -C $(PKG_BUILD_DIR) --strip-components 1 $(TAR_OPTIONS)

define Package/containerd
	SUBMENU:=Golang
	SECTION:=lang
	CATEGORY:=Languages
	TITLE:=containerd v$(PKG_VERSION) - An open reliable container runtime
	URL:=https://containerd.io
	MAINTAINER:=Manfred Gschweidl <m.gschweidl@gmail.com>
	#DEPENDS:=+libpcap +libxml2 +golang-github.com-google-gopacket
	DEPENDS:=runc libseccomp
	#DEFAULT:=n
endef

define Package/cotnainerd/description
	containerd is an industry-standard container runtime with an emphasis
	on simplicity, robustness and portability. It is available as a daemon
	for Linux and Windows, which can manage the complete container lifecycle
	of its host system: image transfer and storage, container execution
	and supervision, low-level storage and network attachments, etc.
endef

#define Build/Prepare
#	$(call Build/GO/Prepare/Default)
#endef

define Build/Compile
	$(call copy_go_source)
	(cd $(GO_PKG_COMPILE_PATH) && make && GO_PKG_BIN_PATH=$(GO_PKG_BIN_PATH) make install)
endef

define Build/InstallDev
	$(call Build/GO/InstallDev/Default,$(1))
endef

define Package/containerd/install
	$(call Package/GO/Install/Default,$(1),,none,none)
endef

$(eval $(call BuildPackage,containerd))
