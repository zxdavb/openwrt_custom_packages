# 
# Copyright (C) 2006-2015 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#
include $(TOPDIR)/rules.mk

PKG_NAME:=bazel
PKG_VERSION:=0.13.1
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://github.com/bazelbuild/bazel/archive/
PKG_HASH:=679c2504475dc2dedd645a3dca72cda2dcb841c90ae834cbda516fbf416628a6

PKG_BUILD_DEPENDS=bazel/host protobuf

include $(INCLUDE_DIR)/host-build.mk
include $(INCLUDE_DIR)/package.mk

UNZIP_CMD=unzip -q -d $(1) $(DL_DIR)/$(PKG_SOURCE)

define Package/bazel
  SECTION:=devel
  CATEGORY:=Development
  TITLE:=bazel
  URL:=https://www.bazel.build
  DEPENDS:=
endef

define Package/bazel/description
 Build system used for some packages
 IMPORTANT NOTE: A JDK needs to be present on the build system
 Currently only JDK version 8 is supported
endef

define Build/Compile
	# platform need to be specified
	# but bazel is a little bit crud... got it working as host tool
	# (cd $(PKG_BUILD_DIR) && ./compile.sh)
	(cd $(PKG_BUILD_DIR) && $(STAGING_DIR_HOSTPKG)/bin/bazel build //src:bazel)
endef

#define Build/Install
#	mkdir $(1)/usr/bin
#	$(CP) $(HOST_BUILD_DIR)/output/bazel $(1)/usr/bin/
#endef

define Package/bazel/install
	$(INSTALL_DIR)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/output/bazel $(1)/usr/bin
endef


$(eval $(call BuildPackage,bazel))
$(eval $(call HostBuild))

