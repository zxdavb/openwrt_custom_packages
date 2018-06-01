#
# This software is licensed under the Public Domain.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=go-bindata
PKG_VERSION:=2018-05-26
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/jteeuwen/go-bindata.git
PKG_SOURCE:=$(PKG_NAME).tar.gz
PKG_SOURCE_VERSION:=6025e8de665b31fa74ab1a66f2cddd8c0abf887e
PKG_HASH:=7b03664484a7811d1dd459ab36b47c33780ad56727b07e9cc9f3c61edb48b56e

PKG_MAINTAINER:=Manfred Gschweidl <m.gschweidl@gmail.com>
PKG_LICENSE_FILES:=LICENSE

#PKG_INSTALL:=1
#PKG_BUILD_DEPENDS:=libprotobuf-c protobuf golang-github.com-google-gopacket

# Default:
# PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)
# DUMP
#DUMP:=1
PKG_USE_GO:=1
PKG_GO_IMPORT_PATH:=github.com/jteeuwen/go-bindata

include $(INCLUDE_DIR)/host-build.mk
include $(INCLUDE_DIR)/package.mk

#go_modules:=github.com/google/gopacket github.com/google/gopacket/layers \
#	github.com/google/gopacket/pcap github.com/google/gopacket/pfring \
#	github.com/google/gopacket/afpacket github.com/gopacket/tcpassembly

#go_modules:=github.com/google/gopacket github.com/google/gopacket/layers \
#	github.com/google/gopacket/pcap

#TAR_CMD:=$(HOST_TAR) -C $(PKG_BUILD_DIR) --strip-components 1 $(TAR_OPTIONS)

define Package/go-bindata
	SUBMENU:=Golang
	SECTION:=lang
	CATEGORY:=Languages
	TITLE:=v$(PKG_VERSION)
	URL:=https://github.com/jteeuwen/go-bindata
	MAINTAINER:=Manfred Gschweidl <m.gschweidl@gmail.com>
	#DEPENDS:=+libpcap +libxml2 +golang-github.com-google-gopacket
	DEPENDS:=
	#DEFAULT:=n
endef

define Package/go-bindata/description
 This package converts any file into managable Go source code. Useful
for embedding binary data into a go program. The file data is
optionally gzip compressed before being converted to a raw byte slice.

It comes with a command line tool in the go-bindata sub directory.
This tool offers a set of command line options, used to customize the
output being generated.
endef

#define Host/Configure
#        $(call perlmod/host/Configure,,,)
#endef

define Host/Compile
	$(call copy_go_source)
	go env
	@echo "Host / Compile"
	@echo "--------------"
	@echo "MAKE MAKECMDGOALS: $(filter-out host host-%,$$(MAKECMDGOALS))"
	@echo "GOPATH: $(GOPATH)"
	@echo "GOARCH: $(GOARCH)"
	@echo "GO_PKG_CROSS_COMPILE: $(GO_PKG_CROSS_COMPILE)"
	@echo "GO_PKG_GOPATH: $(GO_PKG_GOPATH)"
	@echo "GO_PKG_BIN_PATH: $(GO_PKG_BIN_PATH)"
	@echo "GO_PKG_PKG_PATH: $(GO_PKG_PKG_PATH)"
	@echo "GO_PKG_SRC_PATH: $(GO_PKG_SRC_PATH)"
	@echo "GO_PKG_BUILD_PATH: $(GO_PKG_BUILD_PATH)"
	@echo "GO_PKG_COMPILE_PATH: $(GO_PKG_COMPILE_PATH)"
	(cd $(GO_PKG_COMPILE_PATH)/go-bindata && go build && go install)
endef

define Host/Install
	go env
	$(call Build/GO/Host/Install/Default,$(1),,none,none)
endef

#define Build/Prepare
#	$(call Build/GO/Prepare/Default)
#endef

#define Build/Configure
#        $(call perlmod/Configure,,)
#endef

define Build/Compile
	$(call copy_go_source)
	go env
	@echo "Build / Compile"
	@echo "--------------"
	@echo "MAKE MAKECMDGOALS: $(filter-out host host-%,$$(MAKECMDGOALS))"
	@echo "GOPATH: $(GOPATH)"
	@echo "GOARCH: $(GOARCH)"
	@echo "GO_PKG_CROSS_COMPILE: $(GO_PKG_CROSS_COMPILE)"
	@echo "GO_PKG_GOPATH: $(GO_PKG_GOPATH)"
	@echo "GO_PKG_BIN_PATH: $(GO_PKG_BIN_PATH)"
	@echo "GO_PKG_PKG_PATH: $(GO_PKG_PKG_PATH)"
	@echo "GO_PKG_SRC_PATH: $(GO_PKG_SRC_PATH)"
	@echo "GO_PKG_BUILD_PATH: $(GO_PKG_BUILD_PATH)"
	@echo "GO_PKG_COMPILE_PATH: $(GO_PKG_COMPILE_PATH)"
	(cd $(GO_PKG_COMPILE_PATH)/go-bindata && go build && go install)
endef

define Build/InstallDev
	$(call Build/GO/InstallDev/Default,$(1))
endef

define Package/go-bindata/install
	go env
	$(call Package/GO/Install/Default,$(1),,none,none)
endef

$(eval $(call BuildPackage,go-bindata))
$(eval $(call HostBuild))
