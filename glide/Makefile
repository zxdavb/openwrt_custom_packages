#
# This software is licensed under the Public Domain.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=glide
PKG_VERSION:=0.13.1
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/Masterminds/glide.git
PKG_SOURCE_VERSION:=v$(PKG_VERSION)
PKG_HASH:=dcbe71f416480c9f3a2780013a42421d5b3a64cff168617433126481faab645a

PKG_MAINTAINER:=Manfred Gschweidl <m.gschweidl@gmail.com>
PKG_LICENSE_FILES:=LICENSE

PKG_USE_GO:=1
PKG_GO_IMPORT_PATH:=github.com/Masterminds/glide

include $(INCLUDE_DIR)/host-build.mk
include $(INCLUDE_DIR)/package.mk

#TAR_CMD:=$(HOST_TAR) -C $(PKG_BUILD_DIR) --strip-components 1 $(TAR_OPTIONS)

define Package/glide
	SUBMENU:=Golang
	SECTION:=lang
	CATEGORY:=Languages
	TITLE:=v$(PKG_VERSION)
	URL:=https://github.com/jteeuwen/glide
	MAINTAINER:=Manfred Gschweidl <m.gschweidl@gmail.com>
	#DEPENDS:=+libpcap +libxml2 +golang-github.com-google-gopacket
	DEPENDS:=
	#DEFAULT:=n
endef

define Package/glide/description
 This package converts any file into managable Go source code. Useful
for embedding binary data into a go program. The file data is
optionally gzip compressed before being converted to a raw byte slice.

It comes with a command line tool in the glide sub directory.
This tool offers a set of command line options, used to customize the
output being generated.
endef

define Host/Compile
	$(call copy_go_source)
	(cd $(GO_PKG_COMPILE_PATH) && VERSION=$(PKG_SOURCE_VERSION) make build)
endef

define Host/Install
	mkdir -p $(GOPATH)/$(GO_PKG_BIN_PATH)
	$(CP) $(GO_PKG_COMPILE_PATH)/glide $(GOPATH)/$(GO_PKG_BIN_PATH)/
	$(call Build/GO/Host/Install/Default,$(1),,none,none)
endef

define Build/Compile
	$(call copy_go_source)
	( cd $(GO_PKG_COMPILE_PATH) && VERSION=$(PKG_SOURCE_VERSION) make build )
endef

define Build/Install
	mkdir -p $(GOPATH)/$(GO_PKG_BIN_PATH)
	$(CP) $(GO_PKG_COMPILE_PATH)/glide $(GOPATH)/$(GO_PKG_BIN_PATH)/
endef

define Package/glide/install
	$(call Package/GO/Install/Default,$(1),,none,none)
endef

$(eval $(call BuildPackage,glide))
$(eval $(call HostBuild))
