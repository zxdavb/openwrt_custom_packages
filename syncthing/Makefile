#
# This software is licensed under the Public Domain.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=syncthing
PKG_VERSION:=0.14.47
PKG_RELEASE:=1

PKG_SOURCE_PROTO=git
PKG_SOURCE_URL:=https://github.com/syncthing/syncthing.git
PKG_SOURCE_VERSION:=v$(PKG_VERSION)
PKG_SOURCE_DATE:=2018-05-31
PKG_MIRROR_HASH:=bae03345ef49780cbe9f9b9ec78a0426de0cd460ebfa005e729123d5969dadbc

PKG_MAINTAINER:=Manfred Gschweidl <m.gschweidl@gmail.com>
PKG_LICENSE_FILES:=LICENSE

PKG_USE_GO:=1
PKG_GO_IMPORT_PATH:=github.com/syncthing/syncthing

include $(INCLUDE_DIR)/package.mk

#TAR_CMD:=$(HOST_TAR) -C $(PKG_BUILD_DIR) --strip-components 1 $(TAR_OPTIONS)

define Package/syncthing/Default
	SUBMENU:=Golang
	SECTION:=lang
	CATEGORY:=Languages
	TITLE:=syncthing v$(PKG_VERSION)
	URL:=https://containerd.io
	MAINTAINER:=Manfred Gschweidl <m.gschweidl@gmail.com>
	#DEPENDS:=+libpcap +libxml2 +golang-github.com-google-gopacket
	DEPENDS:=
	#DEFAULT:=n
endef

define Package/syncthing
	$(call Package/syncthing/Default)
endef

define Package/syncthing/description
 syncthing server
endef

define Package/syncthing-relay-server
	$(call Package/syncthing/Default)
	TITLE+= - relay server
endef

define Package/syncthing-relay-server/description
 syncthing relay server
endef

define Package/syncthing-relay-pool-server
	$(call Package/syncthing/Default)
	TITLE+=oaschloch 
endef

define Package/syncthing-relay-pool-server/description
 syncthing relay pool server
endef

define Package/syncthing-discovery-server
	$(call Package/syncthing/Default)
	TITLE+= discovery server
endef

define Package/syncthing-discovery-server/description
 syncthing discovery server
endef

#export PATH=$(GOPATH)/bin:$(GOPATH)/$(GO_PKG_BIN_PATH):$(TARGET_PATH_PKG)
define Build/Compile
	$(call copy_go_source)
	( cd $(GO_PKG_COMPILE_PATH) && GOARCH=$(GOHOSTARCH) go run build.go -goarch=$(GOARCH) -no-gobin=true )
endef

#define Build/Install
#	mkdir -p $(GOPATH)/$(GO_PKG_BIN_PATH)
#	$(CP) $(GO_PKG_COMPILE_PATH)/bin/{syncthing,tiller} $(GOPATH)/$(GO_PKG_BIN_PATH)/
#endef

define Package/syncthing/install
	$(call Package/GO/Install/Default,$(1),syncthing,none,none)
endef

define Package/syncthing-relay-server/install
	$(call Package/GO/Install/Default,$(1),strelaysrv,none,none)
endef

define Package/syncthing-relay-pool-server/install
	$(call Package/GO/Install/Default,$(1),strelaypoolsrv,none,none)
endef

define Package/syncthing-discovery-server/install
	$(call Package/GO/Install/Default,$(1),stdiscosrv,none,none)
endef

$(eval $(call BuildPackage,syncthing))
$(eval $(call BuildPackage,syncthing-relay-server))
$(eval $(call BuildPackage,syncthing-relay-pool-server))
$(eval $(call BuildPackage,syncthing-discovery-server))
