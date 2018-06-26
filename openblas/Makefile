# 
# Copyright (C) 2006-2015 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#
include $(TOPDIR)/rules.mk

PKG_NAME:=openblas
PKG_VERSION:=0.3.0
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/xianyi/OpenBLAS.git
PKG_SOURCE_VERSION:=v$(PKG_VERSION)
PKG_SOURCE_DATE:=2018-05-23
PKG_MIRROR_HASH:=adba62c0968bbf032649314e6f322cd83154028950ccfb90ad9494527534b9d9

include $(INCLUDE_DIR)/host-build.mk
include $(INCLUDE_DIR)/package.mk

define Package/openblas
  SECTION:=libs
  CATEGORY:=Libraries
  TITLE:=OpenBLAS
  URL:=https://www.openblas.net
  DEPENDS:=+libgfortran
endef

define Package/openblas/description
OpenBLAS is an optimized BLAS library based on GotoBLAS2
1.13 BSD version.

Note on building: A Fortran compiler is required for
LAPACK (optional).
endef

ifneq (,$(filter $(patsubst "%",%,$(CONFIG_ARCH)),arm))
  ifneq ($(CONFIG_arm_v7),)
    PKG_ARCH=ARMV7
  else ifneq ($(CONFIG_arm_v6),)
    PKG_ARCH=ARMV6
  else ifneq ($(CONFIG_arm_v5),)
    PKG_ARCH=ARMV5
  else ifneq ($(CONFIG_arm_v8),)
    PKG_ARCH=ARMV8
  endif
endif

ifneq (,$(filter $(patsubst "%",%,$(CONFIG_ARCH)),mipsel))
  PKG_ARCH=1004K
endif

define Host/Compile
	$(MAKE) -C $(HOST_BUILD_DIR) \
		BUILD_RELAPACK=1
		FC=gfortran
endef

define Host/Install
	$(MAKE) -C $(HOST_BUILD_DIR) \
		install PREFIX=$(STAGING_DIR_HOSTPKG)
endef

define Build/Compile
	$(MAKE) -C $(PKG_BUILD_DIR)			\
		BUILD_RELAPACK=1			\
		HOSTCC="$(HOSTCC_NOCACHE)"		\
		CC="$(TARGET_CC)"			\
		FC="$(TARGET_CROSS)gfortran"		\
		TARGET=$(PKG_ARCH)
endef

define Build/InstallDev
	# Header
	$(INSTALL_DIR) $(1)/usr/include
	$(CP) $(PKG_BUILD_DIR)/openblas/usr/include/* $(1)/usr/include
	# Libs
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_BUILD_DIR)/openblas/usr/lib/* $(1)/usr/lib
endef

define Build/Install
	$(MAKE) -C $(PKG_BUILD_DIR) \
		install PREFIX=$(1)/usr
endef

define Package/openblas/install
	$(INSTALL_DIR) $(1)/usr
	$(CP) $(PKG_BUILD_DIR)/openblas/usr/{include,lib} $(1)/usr/
endef

$(eval $(call HostBuild))
$(eval $(call BuildPackage,openblas))
