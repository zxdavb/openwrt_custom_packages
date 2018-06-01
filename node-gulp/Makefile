#
# Copyright (C) 2018 Sartura Ltd.
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NPM_NAME:=gulp
PKG_NAME:=node-$(PKG_NPM_NAME)
PKG_VERSION:=4.0.0
PKG_RELEASE:=1

PKG_SOURCE_URL:=https://github.com/gulpjs/gulp/archive/
PKG_SOURCE:=v$(PKG_VERSION).tar.gz
PKG_HASH:=162ca655c3fe251d773da3ab4c4b4501a4fc7323576c0064cda7b5c06f0636d8

HOST_BUILD_DEPENDS:=node/host
PKG_BUILD_DEPENDS:=node/host

PKG_MAINTAINER:=Marko Ratkaj <marko.ratkaj@sartura.hr>
PKG_LICENSE:=MPL-2.0
PKG_LICENSE_FILES:=LICENSE

include $(INCLUDE_DIR)/host-build.mk
include $(INCLUDE_DIR)/package.mk

TAR_CMD=$(HOST_TAR) -C $(1) --strip-components 1 $(TAR_OPTIONS)

define Package/node-gulp
  SUBMENU:=Node.js
  SECTION:=lang
  CATEGORY:=Languages
  TITLE:=Things Gateway by Mozilla
  URL:=https://iot.mozilla.org/gateway/
  DEPENDS:=+node +node-npm
endef

define Package/node-gulp/description
  Build Your Own Web of Things Gateway. The "Web of Things" (WoT) is the
  idea of taking the lessons learned from the World Wide Web and applying
  them to IoT. It's about creating a decentralized Internet of Things by
  giving Things URLs on the web to make them linkable and discoverable,
  and defining a standard data model and APIs to make them interoperable.
endef

CPU:=$(subst powerpc,ppc,$(subst aarch64,arm64,$(subst x86_64,x64,$(subst i386,ia32,$(ARCH)))))

define Host/Compile
	$(MAKE_VARS) \
	$(MAKE_FLAGS) \
	npm_config_arch=$(CPU)
	npm_config_nodedir=$(STAGING_DIR_HOSTPKG)/ \
	npm_config_cache=$(TMP_DIR)/npm-cache \
	npm_config_tmp=$(TMP_DIR)/npm-tmp \
	PREFIX="$(HOST_INSTALL_DIR)/" \
	$(STAGING_DIR_HOSTPKG)/bin/npm install --build-from-source --target_arch=$(CPU) -g $(HOST_BUILD_DIR)
endef

define Host/Install
	$(INSTALL_DIR) $(1)/bin
	$(CP) $(HOST_INSTALL_DIR)/bin/gulp $(1)/bin/

	$(INSTALL_DIR) $(1)/lib/node_modules/gulp
	$(CP) $(HOST_BUILD_DIR)/{bin,docs,node_modules,test,LICENSE,appveyor.yml,index.js,package.json} \
		$(1)/lib/node_modules/gulp
endef

define Build/Compile
	$(MAKE_VARS) \
	$(MAKE_FLAGS) \
	npm_config_arch=$(CONFIG_ARCH) \
	npm_config_nodedir=$(STAGING_DIR)/usr/ \
	npm_config_cache=$(TMP_DIR)/npm-cache \
	npm_config_tmp=$(TMP_DIR)/npm-tmp \
	PREFIX="$(PKG_INSTALL_DIR)/usr/" \
	$(STAGING_DIR_HOSTPKG)/bin/npm install --build-from-source --target_arch=$(CPU) -g $(PKG_BUILD_DIR)
endef

define Package/node-gulp/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(CP) $(PKG_INSTALL_DIR)/usr/bin/gulp $(1)/usr/bin/

	$(INSTALL_DIR) $(1)/usr/lib/node_modules/gulp
	$(CP) $(PKG_BUILD_DIR)/{bin,docs,node_modules,test,LICENSE,appveyor.yml,index.js,package.json} \
		$(1)/usr/lib/node_modules/gulp
	#$(LN) ../constants.js $(1)/opt/mozilla-iot/gateway/src/addons/addon-constants.js
	#$(LN) /tmp/mozilla-iot/gateway/run-app.log $(1)/opt/mozilla-iot/gateway/run-app.log
#
	#$(INSTALL_DIR) $(1)/etc/init.d
	#$(INSTALL_BIN) ./files/gulp.init $(1)/etc/init.d/gulp
endef

$(eval $(call BuildPackage,node-gulp))
$(eval $(call HostBuild))
