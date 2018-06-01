#
# This software is licensed under the Public Domain.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=dep
PKG_VERSION:=0.4.1
PKG_RELEASE:=1

PKG_SOURCE:=v$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://github.com/golang/dep/archive/
PKG_HASH:=df9b050bf4dbb2a8cf04372097a68b04a6ae1986ed7c5086914ac86d74ea9d49

PKG_MAINTAINER:=Manfred Gschweidl <m.gschweidl@gmail.com>
PKG_LICENSE_FILES:=LICENSE

PKG_USE_GO:=1
PKG_GO_IMPORT_PATH:=github.com/golang/dep

include $(INCLUDE_DIR)/host-build.mk
include $(INCLUDE_DIR)/package.mk

define Package/dep
	SUBMENU:=Golang
	SECTION:=lang
	CATEGORY:=Languages
	TITLE:=v$(PKG_VERSION) - a go dependency manager
	URL:=https://github.com/jteeuwen/go-bindata
	MAINTAINER:=Manfred Gschweidl <m.gschweidl@gmail.com>
	#DEPENDS:=+libpcap +libxml2 +golang-github.com-google-gopacket
	DEPENDS:=
	#DEFAULT:=n
endef

define Package/go-bindata/description
 dep is a prototype dependency management tool for Go. It requires
 Go 1.9 or newer to compile. dep is safe for production use.

 dep is the official experiment, but not yet the official tool.
 Check out the Roadmap for more on what this means!
endef

define Host/Compile
	$(call copy_go_source)
	(cd $(GO_PKG_COMPILE_PATH)/cmd/dep && go build && go install)
endef

define Host/Install
	go env
	$(call Build/GO/Host/Install/Default,$(1),,none,none)
endef

define Build/Compile
	$(call copy_go_source)
	(cd $(GO_PKG_COMPILE_PATH)/cmd/dep && go build && go install)
endef

define Build/InstallDev
	$(call Build/GO/InstallDev/Default,$(1))
endef

define Package/dep/install
	go env
	$(call Package/GO/Install/Default,$(1),,none,none)
endef

$(eval $(call BuildPackage,dep))
$(eval $(call HostBuild))
