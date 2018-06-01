#
# This software is licensed under the Public Domain.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=buildifier
PKG_VERSION:=2018-05-26
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/bazelbuild/buildtools.git
PKG_SOURCE:=$(PKG_NAME).tar.gz
PKG_SOURCE_VERSION:=588d90030bc8054b550967aa45a8a8d170deba0b
PKG_HASH:=41e5b34b93485383b32e2af7db92636e84ab25c3d286c4398f5ddb94df1e4bf7

PKG_MAINTAINER:=Manfred Gschweidl <m.gschweidl@gmail.com>
PKG_LICENSE_FILES:=LICENSE

PKG_USE_GO:=1
PKG_GO_IMPORT_PATH:=github.com/bazelbuild/buildtools

include $(INCLUDE_DIR)/host-build.mk
include $(INCLUDE_DIR)/package.mk

define Package/buildifier
	SUBMENU:=Golang
	SECTION:=lang
	CATEGORY:=Languages
	TITLE:=v$(PKG_VERSION)
	URL:=https://github.com/bazelbuild/buildtools
	MAINTAINER:=Manfred Gschweidl <m.gschweidl@gmail.com>
	#DEPENDS:=+libpcap +libxml2 +golang-github.com-google-gopacket
	DEPENDS:=
	#DEFAULT:=n
endef

define Package/buildifier/description
 gdm aims to do as little as possible. It will checkout dependencies
to the local vendor directory and does not require that people use gdm
to build your project. In this way, people can still simply go get your
project and build.

We would recommend that you add vendor to your .gitignore file when using gdm.
endef

define Host/Compile
	$(call copy_go_source)
	(cd $(GO_PKG_COMPILE_PATH)/buildifier && go get)
endef

define Host/Install
	go env
	$(call Build/GO/Host/Install/Default,$(1),,none,none)
endef

define Build/Compile
	$(call copy_go_source)
	(cd $(GO_PKG_COMPILE_PATH)/buildifier && go get)
endef

define Build/InstallDev
	$(call Build/GO/InstallDev/Default,$(1))
endef

define Package/buildifier/install
	go env
	$(call Package/GO/Install/Default,$(1),,none,none)
endef

$(eval $(call BuildPackage,buildifier))
$(eval $(call HostBuild))
