#
# This software is licensed under the Public Domain.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=golang
PKG_VERSION:=1.10.2
PKG_RELEASE:=1

PKG_MAINTAINER:=Manfred Gschweidl <m.gschweidl@gmail.com>
PKG_LICENSE_FILES:=LICENSE

PKG_SOURCE:=go$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://github.com/golang/go/archive/
PKG_HASH:=23066371f3a60e9b1017ca25dd2b9bc6b77aad8e72f25295566ba08f01140c7a

# Default:
# PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)
# DUMP
#DUMP:=1
PKG_USE_GO:=1

GO_TOOLCHAIN_VARIANT=toolchain
empty:=
space:=$(empty) $(empty)
GO_RELEASE_NAME:=$(PKG_NAME)-$(subst $(space),.,$(wordlist 1,2, $(subst .,$(space),$(PKG_VERSION))))

include $(INCLUDE_DIR)/package.mk

TAR_CMD:=$(HOST_TAR) -C $(PKG_BUILD_DIR) --strip-components 1 $(TAR_OPTIONS)
GO_PKG_CONFIG:="/usr/bin/pkg-config"
EXCLUDE_DEP_PATH:="*testdata*"
RSTRIP:=:

define Package/golang
	SUBMENU:=Golang
	SECTION:=lang
	CATEGORY:=Languages
	TITLE:=Golang $(PKG_VERSION) compiler and runtime
	URL:=https://golang.org
	MAINTAINER:=Manfred Gschweidl <m.gschweidl@gmail.com>
	DEPENDS:=+libc +libpthread
	#DEFAULT:=n
endef

define Package/golang/description
	GO compiler and runtime package version $(PKG_VERSION)
endef

# GO_GCFLAGS=$(GO_GCFLAGS) GO_LDFLAGS=$(GO_LDFLAGS)
define Build/Compile
	(cd $(PKG_BUILD_DIR)/src && \
	GOROOT_BOOTSTRAP=$(GOROOT_TOOLCHAIN_FINAL) GOARCH=$(GO_GOARCH) GOOS=$(GO_GOOS) \
		GOROOT_FINAL="/usr/lib/$(GO_RELEASE_NAME)" \
		$(if $(GO_GOMIPS),GOMIPS=$(GO_GOMIPS)) \
		$(if $(GO_GOARM),GOARM=$(GO_GOARM)) \
		$(if $(GO_GO386),GO386=$(GO_GO386)) \
		CGO_ENABLED="$(GO_CGO_ENABLED)" \
		PKG_CONFIG=$(GO_PKG_CONFIG) \
		$(if $(CONFIG_EXTRA_GO_CONFIG_OPTION),GO_DISTFLAGS=$(CONFIG_EXTRA_GO_CONFIG_OPTION)) \
		CC="$(GO_CC)" CXX="$(GO_CXX)" \
		CC_FOR_TARGET="$(GO_CC_FOR_TARGET)" CXX_FOR_TARGET="$(GO_CXX_FOR_TARGET)" \
		./make.bash)
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/lib/$(GO_RELEASE_NAME)/pkg
	$(CP) $(PKG_BUILD_DIR)/pkg/include $(1)/usr/lib/$(GO_RELEASE_NAME)/pkg/

	$(CP) $(PKG_BUILD_DIR)/pkg/$(GO_ARCHITECTURE_PACKAGE) $(1)/usr/lib/$(GO_RELEASE_NAME)/pkg/

	$(CP) $(PKG_BUILD_DIR)/src $(1)/usr/lib/$(GO_RELEASE_NAME)/
	$(CP) $(PKG_BUILD_DIR)/VERSION $(1)/usr/lib/$(GO_RELEASE_NAME)/

	$(INSTALL_DIR) $(1)/usr/lib/$(GO_RELEASE_NAME)/pkg/obj/$(GO_ARCHITECTURE_PACKAGE)
	ln -srf $(1)/usr/lib/$(GO_RELEASE_NAME) $(1)/$(GOROOT_TARGET_FINAL)
	$(INSTALL_DIR) $(1)/$(GO_ADDITIONAL_PACKAGES)/bin
	$(INSTALL_DIR) $(1)/$(GO_ADDITIONAL_PACKAGES)/pkg/$(GO_ARCHITECTURE_PACKAGE)
	$(INSTALL_DIR) $(1)/$(GO_ADDITIONAL_PACKAGES)/src
endef

define Package/golang/install
	$(INSTALL_DIR) $(1)/usr/lib/$(GO_RELEASE_NAME)/bin
	ln -srf $(1)/usr/lib/$(GO_RELEASE_NAME) $(1)/$(GOROOT_TARGET_FINAL)

	$(CP) $(PKG_BUILD_DIR)/doc $(1)/usr/lib/$(GO_RELEASE_NAME)

	$(CP) $(PKG_BUILD_DIR)/favicon.ico $(1)/usr/lib/$(GO_RELEASE_NAME)

	$(INSTALL_DIR) $(1)/usr/lib/$(GO_RELEASE_NAME)/pkg
	$(CP) $(PKG_BUILD_DIR)/pkg/include $(1)/usr/lib/$(GO_RELEASE_NAME)/pkg/

	$(CP) $(PKG_BUILD_DIR)/pkg/$(GO_ARCHITECTURE_PACKAGE) $(1)/usr/lib/$(GO_RELEASE_NAME)/pkg/

	$(INSTALL_DIR) $(1)/usr/lib/$(GO_RELEASE_NAME)/pkg/tool
	$(CP) $(PKG_BUILD_DIR)/pkg/tool/$(GO_ARCHITECTURE_PACKAGE) $(1)/usr/lib/$(GO_RELEASE_NAME)/pkg/tool/

	$(CP) $(PKG_BUILD_DIR)/src $(1)/usr/lib/$(GO_RELEASE_NAME)/
	$(CP) $(PKG_BUILD_DIR)/test $(1)/usr/lib/$(GO_RELEASE_NAME)/
	$(CP) $(PKG_BUILD_DIR)/VERSION $(1)/usr/lib/$(GO_RELEASE_NAME)/

	$(INSTALL_DIR) $(1)/usr/lib/$(GO_RELEASE_NAME)/pkg/obj/$(GO_ARCHITECTURE_PACKAGE)

ifneq ($(GOARCH),$(GOHOSTARCH))
	$(CP) $(PKG_BUILD_DIR)/bin/$(GO_ARCHITECTURE_PACKAGE)/* $(1)/usr/lib/$(GO_RELEASE_NAME)/bin/
else
	$(CP) $(PKG_BUILD_DIR)/bin/* $(1)/usr/lib/$(GO_RELEASE_NAME)/bin/
endif
	$(INSTALL_DIR) $(1)/usr/bin
	ln -srf $(1)/$(GOROOT_TARGET_FINAL)/bin/go $(1)/usr/bin/go
	ln -srf $(1)/$(GOROOT_TARGET_FINAL)/bin/gofmt $(1)/usr/bin/gofmt
	$(INSTALL_DIR) $(1)/$(GO_ADDITIONAL_PACKAGES)/bin
	$(INSTALL_DIR) $(1)/$(GO_ADDITIONAL_PACKAGES)/pkg/$(GO_ARCHITECTURE_PACKAGE)
	$(INSTALL_DIR) $(1)/$(GO_ADDITIONAL_PACKAGES)/src
endef

define Package/golang/postrm
#!/bin/sh
rm -f /usr/bin/go
rm -f /usr/bin/gofmt
rm -rf /usr/lib/go
rm -rf /usr/lib/$(GO_RELEASE_NAME)
#rm -rf $(GO_ADDITIONAL_PACKAGES)
endef

$(eval $(call BuildPackage,golang))
