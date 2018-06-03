#
# This software is licensed under the Public Domain.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=helm
PKG_VERSION:=2.9.1
PKG_RELEASE:=1

PKG_SOURCE_PROTO=git
PKG_SOURCE_URL:=https://github.com/kubernetes/helm.git
PKG_SOURCE_VERSION:=v$(PKG_VERSION)
PKG_SOURCE_DATE:=2018-05-31
PKG_MIRROR_HASH:=98aabade45e681f28fc6238fb7dc7720247b6b0a56e887a11ded3e9f3f51cf70

PKG_MAINTAINER:=Manfred Gschweidl <m.gschweidl@gmail.com>
PKG_LICENSE_FILES:=LICENSE

PKG_USE_GO:=1
PKG_GO_IMPORT_PATH:=k8s.io/helm

PKG_BUILD_DEPENDS:=glide/host

include $(INCLUDE_DIR)/package.mk

#TAR_CMD:=$(HOST_TAR) -C $(PKG_BUILD_DIR) --strip-components 1 $(TAR_OPTIONS)

define Package/helm/Default
	SUBMENU:=Golang
	SECTION:=lang
	CATEGORY:=Languages
	TITLE:=helm v$(PKG_VERSION)
	URL:=https://containerd.io
	MAINTAINER:=Manfred Gschweidl <m.gschweidl@gmail.com>
	#DEPENDS:=+libpcap +libxml2 +golang-github.com-google-gopacket
	DEPENDS:=
	#DEFAULT:=n
endef

define Package/helm-client
 	$(call Package/helm/Default)
	TITLE+= - client
endef

define Package/helm-server
	$(call Package/helm/Default)
	TITLE+= - server
endef

define Package/helm-server/description
 helm server (tiller)
endef

define Package/helm-client/description
 helm client
endef

export PATH=$(GOPATH)/bin:$(GOPATH)/$(GO_PKG_BIN_PATH):$(TARGET_PATH_PKG)
define Build/Compile
	$(call copy_go_source)
	( cd $(GO_PKG_COMPILE_PATH) && PATH=$(PATH) GOARCH=$(GOHOSTARCH) make bootstrap )
	( cd $(GO_PKG_COMPILE_PATH) && PATH=$(PATH) GOARCH=$(GOARCH) make build )
endef

#define Build/Install
#	mkdir -p $(GOPATH)/$(GO_PKG_BIN_PATH)
#	$(CP) $(GO_PKG_COMPILE_PATH)/bin/{helm,tiller} $(GOPATH)/$(GO_PKG_BIN_PATH)/
#endef

define Package/helm-client/install
	$(call Package/GO/Install/Default,$(1),helm,none,none)
endef

define Package/helm-server/install
	$(call Package/GO/Install/Default,$(1),tiller,none,none)
endef

$(eval $(call BuildPackage,helm-client))
$(eval $(call BuildPackage,helm-server))
