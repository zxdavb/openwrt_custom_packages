# 
# Copyright (C) 2006-2015 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#
include $(TOPDIR)/rules.mk

PKG_NAME:=bazel-host
PKG_VERSION:=0.13.1
PKG_RELEASE:=1

PKG_SOURCE:=bazel-$(PKG_VERSION)-dist.zip
PKG_SOURCE_URL:=https://github.com/bazelbuild/bazel/archive/
PKG_HASH:=b0269e75b40d87ff87886e5f3432cbf88f70c96f907ab588e6c21b2922d72db0

HOST_BUILD_DEPENDS=python/host

include $(INCLUDE_DIR)/host-build.mk
include $(INCLUDE_DIR)/package.mk

UNZIP_CMD=unzip -q -d $(1) $(DL_DIR)/$(PKG_SOURCE)

define Package/bazel-host
  SECTION:=devel
  CATEGORY:=Development
  TITLE:=bazel
  URL:=https://www.bazel.build
  DEPENDS:=
endef

define Package/bazel-host/description
 Build system used for some packages
 IMPORTANT NOTE: A JDK needs to be present on the build system
 Currently only JDK version 8 is supported
endef

#define Host/Clean
#	$(call Host/Clean/Default)
#endef

#define Host/Prepare
#	$(call Host/Prepare/Default)
#	$(CP) $(DL_DIR)/$(PKG_SOURCE) $(HOST_BUILD_DIR)
#endef

define Host/Compile
	(cd $(HOST_BUILD_DIR) && bash ./compile.sh)
endef

define Host/Install
	mkdir -p $(1)/bin
	$(CP) $(HOST_BUILD_DIR)/output/bazel $(1)/bin/
endef

define Build/Compile
endef

define Build/Install
endef

define Package/bazel-host/install
endef

$(eval $(call BuildPackage,bazel-host))
$(eval $(call HostBuild))
