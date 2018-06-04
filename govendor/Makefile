#
# This software is licensed under the Public Domain.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=govendor
PKG_VERSION:=1.0.9
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/kardianos/govendor.git
PKG_SOURCE_VERSION:=v$(PKG_VERSION)
PKG_HASH:=156eca0ca540db3d06b4e154226a7640c0eea590093d124d0c8ff776b37dd645

PKG_MAINTAINER:=Manfred Gschweidl <m.gschweidl@gmail.com>
PKG_LICENSE_FILES:=LICENSE

PKG_USE_GO:=1
PKG_GO_IMPORT_PATH:=github.com/kardianos/govendor

include $(INCLUDE_DIR)/host-build.mk
include $(INCLUDE_DIR)/package.mk

define Package/govendor
	SUBMENU:=Golang
	SECTION:=lang
	CATEGORY:=Languages
	TITLE:=v$(PKG_VERSION)
	URL:=https://github.com/kardianos/govendor
	MAINTAINER:=Manfred Gschweidl <m.gschweidl@gmail.com>
	#DEPENDS:=+libpcap +libxml2 +golang-github.com-google-gopacket
	DEPENDS:=
	#DEFAULT:=n
endef

define Package/govendor/description
 govendor aims to do as little as possible. It will checkout dependencies
to the local vendor directory and does not require that people use govendor
to build your project. In this way, people can still simply go get your
project and build.

We would recommend that you add vendor to your .gitignore file when using govendor.
endef

define Host/Compile
	$(call copy_go_source)
	(cd $(GO_PKG_COMPILE_PATH) && go get)
endef

define Host/Install
	$(call Build/GO/Host/Install/Default,$(1),,none,none)
endef

define Build/Compile
	$(call copy_go_source)
	(cd $(GO_PKG_COMPILE_PATH) && go get)
endef

define Build/InstallDev
	$(call Build/GO/InstallDev/Default,$(1))
endef

define Package/govendor/install
	go env
	$(call Package/GO/Install/Default,$(1),,none,none)
endef

$(eval $(call BuildPackage,govendor))
$(eval $(call HostBuild))
