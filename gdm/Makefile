#
# This software is licensed under the Public Domain.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=gdm
PKG_VERSION:=2018-05-26
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/sparrc/gdm.git
PKG_SOURCE:=$(PKG_NAME).tar.gz
PKG_SOURCE_VERSION:=81089dabfa023c18dff6b9c3013467854f9af776
PKG_HASH:=911de0d96b74a96fe51f5f0a167a5f50474b0c7dfa78f0f32a5e6a85e2834f3b

PKG_MAINTAINER:=Manfred Gschweidl <m.gschweidl@gmail.com>
PKG_LICENSE_FILES:=LICENSE

PKG_USE_GO:=1
PKG_GO_IMPORT_PATH:=github.com/sparrc/gdm

include $(INCLUDE_DIR)/host-build.mk
include $(INCLUDE_DIR)/package.mk

define Package/gdm
	SUBMENU:=Golang
	SECTION:=lang
	CATEGORY:=Languages
	TITLE:=v$(PKG_VERSION)
	URL:=https://github.com/sparrc/gdm
	MAINTAINER:=Manfred Gschweidl <m.gschweidl@gmail.com>
	#DEPENDS:=+libpcap +libxml2 +golang-github.com-google-gopacket
	DEPENDS:=
	#DEFAULT:=n
endef

define Package/gdm/description
 gdm aims to do as little as possible. It will checkout dependencies
to the local vendor directory and does not require that people use gdm
to build your project. In this way, people can still simply go get your
project and build.

We would recommend that you add vendor to your .gitignore file when using gdm.
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

define Package/gdm/install
	go env
	$(call Package/GO/Install/Default,$(1),,none,none)
endef

$(eval $(call BuildPackage,gdm))
$(eval $(call HostBuild))
