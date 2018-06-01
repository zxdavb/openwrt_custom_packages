#
# Copyright (C) 2014 Arduino LLC
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NPM_NAME:=yarn
PKG_NAME:=node-$(PKG_NPM_NAME)
PKG_VERSION:=1.7.0
PKG_RELEASE:=1

PKG_SOURCE_URL:=https://github.com/yarnpkg/yarn/archive/
PKG_SOURCE:=v$(PKG_VERSION).tar.gz
PKG_HASH:=2ce1e580d5b583ac43c9d781061e56e4cd2a27628f7cab0d59fe9412c283287e

HOST_BUILD_DEPENDS:=node/host node-gulp/host
PKG_BUILD_DEPENDS:=node/host node-gulp/host
PKG_NODE_VERSION:=8.10.0

PKG_MAINTAINER:=John Crispin <blogic@openwrt.org>
PKG_LICENSE:=MIT
PKG_LICENSE_FILES:=LICENSE.txt

include $(INCLUDE_DIR)/host-build.mk
include $(INCLUDE_DIR)/package.mk

TAR_CMD=$(HOST_TAR) -C $(1) --strip-components 1 $(TAR_OPTIONS)

CPU:=$(subst powerpc,ppc,$(subst aarch64,arm64,$(subst x86_64,x64,$(subst i386,ia32,$(ARCH)))))

#EXTRA_LDFLAGS=-L$(TOOLCHAIN_DIR)/lib/ -Wl,-rpath-link $(TOOLCHAIN_DIR)/lib/

define Package/node-yarn
  SUBMENU:=Node.js
  SECTION:=lang
  CATEGORY:=Languages
  TITLE:=Node.js package for yarn package manager
  URL:=https://yarnpkg.com
  DEPENDS:=+node +node-npm +libstdcpp +libc +librt
endef

define Package/node-yarn/description
 Node.js package to access serial ports for reading and writing OR Welcome your robotic JavaScript overlords. Better yet, program them!
endef

define Host/Compile
	$(MAKE_VARS) \
	$(MAKE_FLAGS) \
	npm_config_arch=$(CONFIG_ARCH) \
	npm_config_nodedir=$(STAGING_DIR_HOSTPKG)/ \
	npm_config_cache=$(TMP_DIR)/npm-cache \
	npm_config_tmp=$(TMP_DIR)/npm-tmp \
	PREFIX="$(HOST_INSTALL_DIR)/" \
	$(STAGING_DIR_HOSTPKG)/bin/npm install --build-from-source --target_arch=$(CPU) -g $(HOST_BUILD_DIR)

	(cd $(HOST_BUILD_DIR) && $(STAGING_DIR_HOSTPKG)/bin/npm link)
	#rm -rf $(HOST_BUILD_DIR)/node_modules/flow-bin/flow-linux*
	
	# $(STAGING_DIR_HOSTPKG)/bin/gulp -f $(HOST_BUILD_DIR)/gulpfile.js --tasks
	$(STAGING_DIR_HOSTPKG)/bin/gulp -LLLL -f $(HOST_BUILD_DIR)/gulpfile.js build

	$(STAGING_DIR_HOSTPKG)/bin/npm unlink --global yarn
endef

define Host/Install
	$(INSTALL_DIR) $(1)/bin
	$(CP) $(HOST_INSTALL_DIR)/bin/{yarn,yarnpkg} $(1)/bin/

	$(INSTALL_DIR) $(1)/lib/node_modules/yarn
	$(CP) $(HOST_BUILD_DIR)/{bin,lib,node_modules,LICENSE,package.json} \
		$(1)/lib/node_modules/yarn
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

	(cd $(PKG_BUILD_DIR) && $(STAGING_DIR_HOSTPKG)/bin/npm link)
	rm -rf $(PKG_BUILD_DIR)/node_modules/flow-bin/flow-linux*
	
	# $(STAGING_DIR_HOSTPKG)/bin/gulp -f $(PKG_BUILD_DIR)/gulpfile.js --tasks
	$(STAGING_DIR_HOSTPKG)/bin/gulp -LLLL -f $(PKG_BUILD_DIR)/gulpfile.js build

	$(STAGING_DIR_HOSTPKG)/bin/npm unlink --global yarn
endef

define Package/node-yarn/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(CP) $(PKG_INSTALL_DIR)/usr/bin/{yarn,yarnpkg} $(1)/usr/bin/

	$(INSTALL_DIR) $(1)/usr/lib/node_modules/yarn
	$(CP) $(PKG_BUILD_DIR)/{bin,lib,node_modules,LICENSE,package.json} \
		$(1)/usr/lib/node_modules/yarn
endef

$(eval $(call BuildPackage,node-yarn))
$(eval $(call HostBuild))
