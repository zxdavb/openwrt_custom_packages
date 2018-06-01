# 
# Copyright (C) 2006-2015 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#
include $(TOPDIR)/rules.mk

PKG_NAME:=envoy
PKG_VERSION:=1.6.0
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/envoyproxy/envoy.git
#PKG_SOURCE_VERSION:=4dd49d8809f7aaa580538b3c228dd99a2fae92a4
PKG_SOURCE_VERSION:=v$(PKG_VERSION)
PKG_SOURCE_DATE:=2018-05-30
PKG_MIRROR_HASH:=3fc7496127da4c99c0d356dc9e9bde8c6837e3939d693a8568265fb0a8ab2145

PKG_BUILD_DEPENDS=bazel-host/host

include $(INCLUDE_DIR)/package.mk

#UNZIP_CMD=unzip -q -d $(1) $(DL_DIR)/$(PKG_SOURCE)

define Package/envoy
  SECTION:=net
  CATEGORY:=Network
  TITLE:=envoy
  URL:=https://www.envoyproxy.io
  DEPENDS:=
endef

define Package/envoy/description
 Build system used for some packages
 IMPORTANT NOTE: A JDK needs to be present on the build system
 Currently only JDK version 8 is supported
endef

define Build/Compile
	(cd $(PKG_BUILD_DIR) && bazel fetch //source/...)
	(cd $(PKG_BUILD_DIR) && bazel build //source/exe:envoy-static)
endef

define Build/Install
	echo "VERSCHISSENDE DRECKSAU"
endef

define Package/envoy/install
	echo "SAUHUND"
endef

$(eval $(call BuildPackage,envoy))
