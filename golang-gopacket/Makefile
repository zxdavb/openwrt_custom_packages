#
# This software is licensed under the Public Domain.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=golang-gopacket
PKG_VERSION:=1.1.14
PKG_RELEASE:=1

PKG_MAINTAINER:=Manfred Gschweidl <m.gschweidl@gmail.com>
PKG_LICENSE_FILES:=LICENSE

PKG_SOURCE:=v1.1.14.tar.gz
PKG_SOURCE_URL:=https://github.com/google/gopacket/archive/
PKG_HASH:=cb67f185cd2547652b9161829e82ebdeafc8984e827825647fd8d3f4e9f93250

# Default:
# PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)
# DUMP
DUMP:=
PKG_USE_GO:=1

include $(INCLUDE_DIR)/package.mk

#go_pkg_lastword=$(lastword $(subst -, ,$(PKG_NAME)))
#go_pkg_import_path:=$(subst -,/,$(PKG_NAME:golang-%=%))
#go_pkg_gopath:=$(PKG_BUILD_DIR)/PKG_GOPATH
#go_modules:=github.com/google/gopacket github.com/google/gopacket/layers \
#	github.com/google/gopacket/pcap github.com/google/gopacket/pfring \
#	github.com/google/gopacket/afpacket github.com/gopacket/tcpassembly

go_modules:=github.com/google/gopacket github.com/google/gopacket/layers \
	github.com/google/gopacket/pcap

TAR_CMD:=$(HOST_TAR) -C $(PKG_BUILD_DIR) --strip-components 1 $(TAR_OPTIONS)
#PKG_CONFIG:="/usr/bin/pkg-config"
#EXCLUDE_DEP_PATH:="*testdata*"
RSTRIP:=:

#export GOROOT=$(GOROOT_TOOLCHAIN_FINAL)
#export GOPATH=$(go_pkg_gopath)
#export GOARCH=$(GO_GOARCH)
#export GOOS=$(GO_GOOS)
#export GOMIPS=$(GO_GOMIPS)

go_pkg_import_path:=github.com/google/gopacket

define Package/golang-gopacket
	SUBMENU:=Golang
	SECTION:=lang
	CATEGORY:=Languages
	TITLE:=Golang gopacket V$(PKG_VERSION) library
	URL:=https://github.com/google/gopacket
	MAINTAINER:=Manfred Gschweidl <m.gschweidl@gmail.com>
	DEPENDS:=+libpcap
	#DEFAULT:=n
endef

define Package/golang-gopacket/description
	Provides packet processing capabilities for Go - V$(PKG_VERSION)
	* layers: You'll probably use this every time.  This contains of the logic
	  built into gopacket for decoding packet protocols.  Note that all example
	  code below assumes that you have imported both gopacket and
	  gopacket/layers.
	* pcap: C bindings to use libpcap to read packets off the wire.
	* pfring: C bindings to use PF_RING to read packets off the wire.
	* afpacket: C bindings for Linux's AF_PACKET to read packets off the wire.
	* tcpassembly: TCP stream reassembly
endef

define Package/golang-gopacket/install
	mkdir -p /home/manfred/DRECKSAU_1
endef

define Package/golang-gopacket/preinst
#!/bin/sh
mkdir -p /home/manfred/SCHEISS_DRECKSAU
endef

define Build/Compile
	$(call Build/GO/Compile)
	go build -x -v -work github.com/google/gopacket
endef

#ifdef Package/$(PGK_NAME)/install
#	$(info "SCHEISSS DRECKSAU!!!!!!!!!!!!!!!!!!!!!")
#endif

define Build/InstallDev
	$(INSTALL_DIR) $(1)$(GO_ADDITIONAL_PACKAGES)
	$(CP) $(go_pkg_gopath)/* $(1)$(GO_ADDITIONAL_PACKAGES)
endef

$(eval $(call BuildPackage,golang-gopacket))

