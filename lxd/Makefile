#
# This software is licensed under the Public Domain.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=lxd
PKG_VERSION:=3.0.0
PKG_RELEASE:=1

PKG_MAINTAINER:=Manfred Gschweidl <m.gschweidl@gmail.com>
PKG_LICENSE_FILES:=LICENSE

#PKG_SOURCE_URL:=https://linuxcontainers.org/downloads/lxd/
#PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSIcdON).tar.gz
#PKG_HASH:=9c13f1bfbec68021c8e2264bcbcdf1f53de693256affa0f92a134d9d5ccbb754

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/lxc/lxd.git
PKG_SOURCE_VERSION:=lxd-3.0.0
PKG_SOURCE_DATE:=2018-05-02
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
#PKG_SOURCE_VERSION:=xxxCOMMIT-HASHxxx
PKG_MIRROR_HASH:=285fb38aed9445a7a5759480e26e389a31bbcd1694690a703030b7cb15cab138
#PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)

#PKG_INSTALL:=0
PKG_BUILD_DEPENDS:=acl dnsmasq git lxc make pkg-config rsync \
	squashfs-tools tar xz \
	libseccomp libcap \
	lvm2 btrfs-progs

# Default:
# PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)
# DUMP
#DUMP:=1
PKG_USE_GO:=1
PKG_GO_IMPORT_PATH:=github.com/lxc/lxd

include $(INCLUDE_DIR)/package.mk

#export PKG_CONFIG_PATH=$(PKG_CONFIG_PATH):$(STAGING_DIR)/usr/lib/pkgconfig
#export PKG_CONFIG_PATH=$(STAGING_DIR)/usr/lib/pkgconfig
#$(info $(PKG_CONFIG_PATH))

#go_modules:=github.com/google/gopacket github.com/google/gopacket/layers \
#	github.com/google/gopacket/pcap github.com/google/gopacket/pfring \
#	github.com/google/gopacket/afpacket github.com/gopacket/tcpassembly

#go_modules:=github.com/CanonicalLtd/raft-test/internal/raftext github.com/golang/protobuf/proto/testdata

TAR_CMD:=$(HOST_TAR) -C $(PKG_BUILD_DIR) --strip-components 1 $(TAR_OPTIONS)

define Package/lxd/Default
	SECTION:=lang
	CATEGORY:=Languages
	SUBMENU:=Golang
	TITLE:=LXD v$(PKG_VERSION) - System container manager
	URL:=https://linuxcontainers.org/lxd/
	MAINTAINER:=Manfred Gschweidl <m.gschweidl@gmail.com>
	#DEFAULT:=n
endef

define Package/lxd-server
$(call Package/lxd/Default)
  TITLE+= server
  DEPENDS=+libacl +acl +lxc +liblxc \
	  +squashfs-tools-unsquashfs +xz +xz-utils \
	  +shadow-newuidmap +shadow-newgidmap +tar \
	  +rsync
endef

define Package/lxd-server/description
	System container manager using linux containers
	LXD daemon (server) package

	NOTE: LXD requires dnsmasq with ipv6 support for dhcp.
	dnsmasq-dhcpv6 or dnsmasq-full
endef

define Package/lxd-client
$(call Package/lxd/Default)
  TITLE+= client
endef

define Package/lxd-client/description
	System container manager using linux containers
	LXD client tools
endef

#define Build/Prepare
#	$(call Build/GO/Prepare/Default)
#endef

define Build/Compile
	$(call copy_go_source)
	(cd $(GO_PKG_COMPILE_PATH) && make)
endef

define Build/InstallDev
	echo "LXD InstallDev"
	$(call Build/GO/InstallDev/Default,$(1))
endef

#define Build/Install
#	# nothing to do
#endef

define Package/lxd-server/install
	echo "LXD server"
	$(call Package/GO/Install/Default,$(1),lxd lxd-benchmark,none,none)
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/lxd.init $(1)/etc/init.d/lxd
endef

define Package/lxd-client/install
	echo "LXD client"
	$(call Package/GO/Install/Default,$(1),lxc lxd-p2c,none,none)
endef

#### Install / Uninstall scripts
# server postinstall
define Package/lxd-server/postinst
#!/bin/sh
echo "Create symlinks for server binaries..."
ln -sf $(GO_PACKAGES_GOPATH)/bin/lxd /usr/sbin/lxd
ln -sf $(GO_PACKAGES_GOPATH)/bin/lxd-benchmark /usr/bin/lxd-benchmark
echo "Create 'sub{u,g}ids' for 'root' and 'lxd' (LXD can run unprivileged containers)..."
echo "root:1000000:1000000000" | tee -a /etc/subuid /etc/subgid
echo "lxd:1000000:1000000000" | tee -a /etc/subuid /etc/subgid
echo "HINT: the '--group' sudo bit allows everyone to talk to LXD in the 'sudo' group"
echo "IMPORTANT: LXD requires dnsmasq with dhcp ipv6 support for lxd-bridge"
echo "           ('dnsmasq-dhcpv6' or 'dnsmasq-full')"
endef

# client postinstall
define Package/lxd-client/postinst
#!/bin/sh
echo "Create symlinks for user binaries..."
ln -sf $(GO_PACKAGES_GOPATH)/bin/lxc /usr/bin/lxc
ln -sf $(GO_PACKAGES_GOPATH)/bin/lxd-p2c /usr/bin/lxd-p2c
endef

# server postrm
define Package/lxd-server/postrm
#!/bin/sh
echo "Remove symlinks for server binaries..."
rm -f /usr/sbin/lxd
rm -f /usr/bin/lxd-benchmark
echo "IMPORTANT: 'sub{u,g}ids' are not removed for 'root' and 'lxd' (can be done manually)"
endef

# client postrm
define Package/lxd-client/postrm
#!/bin/sh
echo "Remove symlinks for client binaries..."
rm -f /usr/bin/lxc
rm -f /usr/bin/lxd-p2c
endef

$(eval $(call BuildPackage,lxd-server))
$(eval $(call BuildPackage,lxd-client))
