#
# This software is licensed under the Public Domain.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=docker
PKG_VERSION:=17.03.2-ce
PKG_RELEASE:=1

PKG_SOURCE_PROTO=git
PKG_SOURCE_URL:=https://github.com/moby/moby.git
PKG_SOURCE_VERSION:=v$(PKG_VERSION)
PKG_SOURCE_DATE:=2018-05-31
PKG_MIRROR_HASH:=aa7a42b7bde65fb8e1db01f1c52bf8c82055a605c6e9838f625defa34db34ef0

PKG_MAINTAINER:=Manfred Gschweidl <m.gschweidl@gmail.com>
PKG_LICENSE_FILES:=LICENSE

PKG_USE_GO:=1
PKG_GO_IMPORT_PATH:=github.com/docker/docker

include $(INCLUDE_DIR)/package.mk

#PKG_BUILD_DEPENDS:=libseccomp btrfs-progs libdevmapper lvm2

#TAR_CMD:=$(HOST_TAR) -C $(PKG_BUILD_DIR) --strip-components 1 $(TAR_OPTIONS)

define Package/docker/Default
	SUBMENU:=Golang
	SECTION:=lang
	CATEGORY:=Languages
	TITLE:=docker v$(PKG_VERSION)
	URL:=https://containerd.io
	MAINTAINER:=Manfred Gschweidl <m.gschweidl@gmail.com>
	# DEPENDS:=+iptables +e2fsprogs +btrfs-progs +xz-utils +xfs-mkfs +libdevmapper
	#DEFAULT:=n
endef

define Package/docker-client
 	$(call Package/docker/Default)
	TITLE+= - client
endef

define Package/docker-server
	$(call Package/docker/Default)
	TITLE+= - server
	DEPENDS:=+libseccomp +libdevmapper
endef

define Package/docker-server/description
 docker server (tiller)
endef

define Package/docker-client/description
 docker client
endef

define Build/Compile
	$(call copy_go_source)
	( cd $(GO_PKG_COMPILE_PATH) && patch -p0 < vendor-patches/golang.org-x-sys.patch )
	( cd $(GO_PKG_COMPILE_PATH) && patch -p0 < vendor-patches/netns.patch )
	( cd $(GO_PKG_COMPILE_PATH) && DOCKER_GITCOMMIT=f5ec1e2 DOCKER_BUILDTAGS='seccomp exclude_driver_aufs' ./hack/make.sh binary )
endef

define Build/Install
	mkdir -p $(GOPATH)/$(GO_PKG_BIN_PATH)
	$(CP) -P $(GO_PKG_COMPILE_PATH)/bundles/latest/{binary-client,binary-daemon}/* $(GOPATH)/$(GO_PKG_BIN_PATH)/
endef

define Package/docker-client/install
	$(call Package/GO/Install/Default,$(1),docker docker-$(PKG_VERSION),none,none)
endef

define Package/docker-server/install
	$(call Package/GO/Install/Default,$(1),dockerd dockerd-$(PKG_VERSION),none,none)
endef

$(eval $(call BuildPackage,docker-client))
$(eval $(call BuildPackage,docker-server))
