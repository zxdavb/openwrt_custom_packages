#
# This software is licensed under the Public Domain.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=gx-go
PKG_VERSION:=1.7.0
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/whyrusleeping/gx-go.git
PKG_SOURCE_VERSION:=v$(PKG_VERSION)
PKG_SOURCE_DATE:=2018-05-30
PKG_MIRROR_HASH:=ecb58a08f08da91ccb74534638f0629ac27e20995319f8bf7e1aa09fa375c16a

PKG_MAINTAINER:=Manfred Gschweidl <m.gschweidl@gmail.com>
PKG_LICENSE_FILES:=LICENSE

PKG_USE_GO:=1
PKG_GO_IMPORT_PATH:=github.com/whyrusleeping/gx-go

include $(INCLUDE_DIR)/host-build.mk
include $(INCLUDE_DIR)/package.mk

define Package/gx-go
	SUBMENU:=Golang
	SECTION:=lang
	CATEGORY:=Languages
	TITLE:=v$(PKG_VERSION)
	URL:=https://github.com/sparrc/gx-go
	MAINTAINER:=Manfred Gschweidl <m.gschweidl@gmail.com>
	#DEPENDS:=+libpcap +libxml2 +golang-github.com-google-gopacket
	DEPENDS:=
	#DEFAULT:=n
endef

define Package/gx-go/description
 gx-go aims to do as little as possible. It will checkout dependencies
to the local vendor directory and does not require that people use gx-go
to build your project. In this way, people can still simply go get your
project and build.

We would recommend that you add vendor to your .gitignore file when using gx-go.
endef

define Host/Compile
	$(call copy_go_source)
	(cd $(GO_PKG_COMPILE_PATH) && go get)
endef

define Host/Install
	go env
	$(call Build/GO/Host/Install/Default,$(1),,none,none)
endef

define Build/Compile
	$(call copy_go_source)
	(cd $(GO_PKG_COMPILE_PATH) && go get)
endef

#define Build/InstallDev
#	$(call Build/GO/InstallDev/Default,$(1))
#endef

define Package/gx-go/install
	go env
	$(call Package/GO/Install/Default,$(1),,none,none)
endef

$(eval $(call BuildPackage,gx-go))
$(eval $(call HostBuild))
