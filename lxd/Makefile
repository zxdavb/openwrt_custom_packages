#
# This software is licensed under the Public Domain.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=lxd
PKG_VERSION:=3.0.0
PKG_RELEASE:=1

PKG_MAINTAINER:=Manfred Gschweidl <m.gschweidl@gmail.com>
PKG_LICENSE_FILES:=LICENSE

PKG_SOURCE_URL:=https://linuxcontainers.org/downloads/lxd/
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_HASH:=9c13f1bfbec68021c8e2264bcbcdf1f53de693256affa0f92a134d9d5ccbb754

#PKG_INSTALL:=1
PKG_BUILD_DEPENDS:=+acl +dnsmasq +git +golang +lxc +make +pkg-config +rsync \
	+squashfs-tools +tar +xz \
	+libseccomp +libcap \
	+lvm2 +btrfs-progs

# Default:
# PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)
# DUMP
#DUMP:=1
PKG_USE_GO:=1
PKG_GO_IMPORT_PATH:=github.com/lxc/lxd

include $(INCLUDE_DIR)/package.mk

#go_modules:=github.com/google/gopacket github.com/google/gopacket/layers \
#	github.com/google/gopacket/pcap github.com/google/gopacket/pfring \
#	github.com/google/gopacket/afpacket github.com/gopacket/tcpassembly

#go_modules:=github.com/google/gopacket github.com/google/gopacket/layers \
#	github.com/google/gopacket/pcap

#TAR_CMD:=$(HOST_TAR) -C $(PKG_BUILD_DIR) --strip-components 1 $(TAR_OPTIONS)

define Package/lxd
	SUBMENU:=Golang
	SECTION:=lang
	CATEGORY:=Languages
	TITLE:=LXD v$(PKG_VERSION) - System container manager
	URL:=https://linuxcontainers.org/lxd/
	MAINTAINER:=Manfred Gschweidl <m.gschweidl@gmail.com>
	#DEPENDS:=+acl
	#DEFAULT:=n
endef

define Package/golang-github.com-smira-aptly/description
	System container manager using linux containers
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

define Package/lxd/Install
	$(call Package/GO/Install/Default,$(1))
endef

$(eval $(call BuildPackage,lxd))

