#
# This software is licensed under the Public Domain.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=golang-github.com-skydive-project-skydive
PKG_VERSION:=2017-11-29
PKG_RELEASE:=1
PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=http://lxd-phabricator03.lxd.mammoth.home/source/skydive_local.git
PKG_SOURCE:=$(PKG_NAME).tar.gz
PKG_SOURCE_VERSION:=HEAD
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-temp
PKG_HASH:=2010bad900abd0cac921ea9d1fa157ec121362562919b6f1deec85d96e6536c4

PKG_MAINTAINER:=Manfred Gschweidl <m.gschweidl@gmail.com>
PKG_LICENSE_FILES:=LICENSE

PKG_INSTALL:=1
PKG_BUILD_DEPENDS:=libprotobuf-c protobuf

# Default:
# PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)
# DUMP
#DUMP:=1
PKG_USE_GO:=1
PKG_GO_IMPORT_PATH:=github.com/skydive-project/skydive

include $(INCLUDE_DIR)/package.mk

PKG_BUILD_DIR_ORIG:=$(PKG_BUILD_DIR)
TAR_CMD:=$(HOST_TAR) -C $(PKG_BUILD_DIR_ORIG)/src/$(PKG_GO_IMPORT_PATH) --strip-components 1 $(TAR_OPTIONS)

PKG_BUILD_DIR:=$(PKG_BUILD_DIR_ORIG)/src/$(PKG_GO_IMPORT_PATH)
export GOPATH:=$(PKG_BUILD_DIR_ORIG)

define Package/golang-github.com-skydive-project-skydive
	SUBMENU:=Golang
	SECTION:=lang
	CATEGORY:=Languages
	TITLE:=Golang Skydive network analyzer V$(PKG_VERSION) (local)
	URL:=http://lxd-phabricator03.lxd.mammoth.home/source/skydive_local.git
	MAINTAINER:=Manfred Gschweidl <m.gschweidl@gmail.com>
	DEPENDS:=+libpcap +libxml2 +golang-github.com-google-gopacket
	#DEFAULT:=n
endef

define Package/golang-github.com-skydive-project-skydive/description
	Network analyzer
endef

#define Build/Prepare
#	$(call Build/GO/Prepare/Default)
#endef

define Build/Compile
	(cd $(PKG_BUILD_DIR) ; \
		patch -p0 < $(PKG_BUILD_DIR)/patches/Openwrt.Makefile.patch && \
		patch -p0 < $(PKG_BUILD_DIR)/patches/Openwrt.contrib.snort.Makefile.patch && \
		make install ; )
endef

define Build/InstallDev
	$(call Build/GO/InstallDev/Default,$(1))
#	$(INSTALL_DIR) $(1)$(GO_ADDITIONAL_PACKAGES)
#
#	( if [ -d "$(PKG_BUILD_DIR_ORIG)/bin" ]; then \
#	  mkdir -p $(1)$(GO_ADDITIONAL_PACKAGES)/bin ; \
#	  cp -fpR $(PKG_BUILD_DIR_ORIG)/bin/* $(1)$(GO_ADDITIONAL_PACKAGES)/bin/ ; \
#	fi )
#
#	( if [ -d "$(PKG_BUILD_DIR_ORIG)/pkg" ]; then \
#	  mkdir -p $(1)$(GO_ADDITIONAL_PACKAGES)/pkg ; \
# 	  cp -fpR $(PKG_BUILD_DIR_ORIG)/pkg/* $(1)$(GO_ADDITIONAL_PACKAGES)/pkg/ ; \
#	fi )
#
#	( if [ -d "$(PKG_BUILD_DIR_ORIG)/src" ]; then \
#	  mkdir -p $(1)$(GO_ADDITIONAL_PACKAGES)/src ; \
#	  cp -fpR $(PKG_BUILD_DIR_ORIG)/src/* $(1)$(GO_ADDITIONAL_PACKAGES)/src/ ; \
#	fi )
endef

define Package/golang-github.com-skydive-skydive-skydive/install
	$(INSTALL_DIR) $(1)$(GO_ADDITIONAL_PACKAGES)

	( if [ -d "$(PKG_BUILD_DIR_ORIG)/bin" ] && \
	  [ -e "$(PKG_BUILD_DIR_ORIG)/bin/skydive" ]; then \
	  mkdir -p $(1)$(GO_ADDITIONAL_PACKAGES)/bin ; \
  	  cp -fpR $(PKG_BUILD_DIR_ORIG)/bin/skydive $(1)$(GO_ADDITIONAL_PACKAGES)/bin/ ; \
  	  cp -fpR $(PKG_BUILD_DIR_ORIG)/bin/snort $(1)$(GO_ADDITIONAL_PACKAGES)/bin/ ; \
  	fi )

	( if [ -d "$(PKG_BUILD_DIR_ORIG)/bin/$(GOOS)_$(GOARCH)" ] && \
	  [ -e "$(PKG_BUILD_DIR_ORIG)/bin/$(GOOS)_$(GOARCH)/skydive" ]; then \
	  mkdir -p $(1)$(GO_ADDITIONAL_PACKAGES)/bin ; \
  	  cp -fpR $(PKG_BUILD_DIR_ORIG)/bin/$(GOOS)_$(GOARCH)/skydive $(1)$(GO_ADDITIONAL_PACKAGES)/bin/ ; \
  	  cp -fpR $(PKG_BUILD_DIR_ORIG)/bin/$(GOOS)_$(GOARCH)/snort $(1)$(GO_ADDITIONAL_PACKAGES)/bin/ ; \
  	fi )

	( if [ -d "$(PKG_BUILD_DIR_ORIG)/pkg/$(GOOS)_$(GOARCH)" ]; then \
	  mkdir -p $(1)$(GO_ADDITIONAL_PACKAGES)/pkg/$(GOOS)_$(GOARCH)/github.com ; \
  	  cp -fpR $(PKG_BUILD_DIR_ORIG)/pkg/$(GOOS)_$(GOARCH)/github.com/golang \
	    $(1)$(GO_ADDITIONAL_PACKAGES)/pkg/$(GOOS)_$(GOARCH)/github.com ; \
   	  cp -fpR $(PKG_BUILD_DIR_ORIG)/pkg/$(GOOS)_$(GOARCH)/github.com/skydive-project \
	    $(1)$(GO_ADDITIONAL_PACKAGES)/pkg/$(GOOS)_$(GOARCH)/github.com ; \
 	fi )

	( if [ -d "$(PKG_BUILD_DIR_ORIG)/src" ]; then \
	  mkdir -p $(1)$(GO_ADDITIONAL_PACKAGES)/src/github.com ; \
  	  cp -fpR $(PKG_BUILD_DIR_ORIG)/src/github.com/golang $(1)$(GO_ADDITIONAL_PACKAGES)/src/github.com ; \
  	  cp -fpR $(PKG_BUILD_DIR_ORIG)/src/github.com/skydive-project $(1)$(GO_ADDITIONAL_PACKAGES)/src/github.com ; \
  	fi )
endef

define Build/Clean
	$(call Build/Clean/Default)
	rm -rf $(PKG_BUILD_DIR_ORIG)
endef

$(eval $(call BuildPackage,golang-github.com-skydive-project-skydive))

