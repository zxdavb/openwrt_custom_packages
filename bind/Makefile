#
# Copyright (C) 2006-2012 OpenWrt.org
#               2014-2017 Noah Meyerhans <frodo@morgul.net>
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=bind
#PKG_NAME_ORIG:=bind
PKG_VERSION:=9.11.1
PKG_RELEASE:=3
USERID:=bind=57:bind=57

PKG_MAINTAINER:=Manfred Gschweidl <m.gschweidl@gmail.com>
PKG_LICENSE := BSD-3-Clause

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:= \
	ftp://ftp.isc.org/isc/bind9/$(PKG_VERSION) \
	http://www.mirrorservice.org/sites/ftp.isc.org/isc/bind9/$(PKG_VERSION)
PKG_MD5SUM:=c384ab071d902bac13487c1268e5a32f

PKG_FIXUP:=autoreconf
PKG_REMOVE_FILES:=aclocal.m4 libtool.m4

PKG_INSTALL:=1
PKG_USE_MIPS16:=0

PKG_CONFIG_DEPENDS := \
	CONFIG_OPENSSL_WITH_EC \
	CONFIG_BIND_ENABLE_FILTER_AAAA

include $(INCLUDE_DIR)/package.mk
# Added by Mammoth for bind-dnssec-python install - $(PYTHON_PKG_PATH)
$(call include_mk,python-package.mk)

define Package/bind/Default
  SECTION:=net
  CATEGORY:=Network
  DEPENDS:=+bind-libs
  TITLE:=bind
  URL:=https://www.isc.org/software/bind
  SUBMENU:=IP Addresses and Names
endef

define Package/bind-libs
  SECTION:=libs
  CATEGORY:=Libraries
  DEPENDS:=+libopenssl +zlib
  TITLE:=bind shared libraries
  URL:=https://www.isc.org/software/bind
endef

define Package/bind-server
  $(call Package/bind/Default)
  TITLE+= DNS server
endef

define Package/bind-server/config
	source "$(SOURCE)/Config.in"
endef

define Package/bind-client
  $(call Package/bind/Default)
  TITLE+= dynamic DNS client
endef

define Package/bind-tools
  $(call Package/bind/Default)
  TITLE+= administration tools (all - Added by Mammoth: delv and nsupdate)
endef

define Package/bind-rndc
  $(call Package/bind/Default)
  TITLE+= administration tools (rndc and rndc-confgen only)
endef

# Additional Tools - Package added by Mammoth
define Package/bind-ddns
  $(call Package/bind/Default)
  TITLE+= administration tools (ddns-confgen and tsig-keygen only) - Package added by Mammoth
endef

# Additional Tools - Package added by Mammoth
define Package/bind-dnssec-python
  $(call Package/bind/Default)
  DEPENDS:=+python-light +python-codecs +python-logging +python-openssl +python-ply
  TITLE+= administration tools - REQUIRE PYTHON (ddns-checkds, ddns-coverage and ddns-keymgr only) - Package added by Mammoth
endef

define Package/bind-check
  $(call Package/bind/Default)
  TITLE+= administration tools (named-checkconf and  named-checkzone only - Added by Mammoth: named-compilezone and named-journalprint)
endef

define Package/bind-dnssec
  $(call Package/bind/Default)
  TITLE+= administration tools (dnssec-keygen and dnssec-signzone only - Added by Mammoth dnssec-dsfromkey, dnssec-importkey, dnssec-keyfromlabel \
	  dnssec-revoke, dnssec-settime and dnssec-verify)
endef

define Package/bind-host
  $(call Package/bind/Default)
  TITLE+= simple DNS client
endef

define Package/bind-dig
  $(call Package/bind/Default)
  TITLE+= DNS excavation tool
endef

export BUILD_CC="$(TARGET_CC)"

CONFIGURE_ARGS += \
	--enable-shared \
	--enable-static \
	--with-randomdev="/dev/urandom" \
	--disable-linux-caps \
	--with-openssl="$(STAGING_DIR)/usr" \
	--with-libjson=no \
	--with-libtool \
	--with-libxml2=no \
	--enable-epoll=yes \
	--with-gost=no \
	--with-gssapi=no \
	--with-ecdsa=$(if $(CONFIG_OPENSSL_WITH_EC),yes,no) \
	--with-readline=no \
	--sysconfdir=/etc/bind \

ifdef CONFIG_BIND_ENABLE_FILTER_AAAA
	CONFIGURE_ARGS += \
		--enable-filter-aaaa
endif

CONFIGURE_VARS += \
	BUILD_CC="$(TARGET_CC)" \

define Build/Compile
	$(MAKE) -C $(PKG_BUILD_DIR)/lib/dns \
		BUILD_CC="$(HOSTCC)" \
		CC="$(HOSTCC)" \
		CFLAGS="-O2" \
		LIBS="" \
		gen
	$(call Build/Compile/Default)
endef

define Build/InstallDev
	# Header
	$(INSTALL_DIR) $(1)/usr/include/
	$(CP) $(PKG_INSTALL_DIR)/usr/include/* $(1)/usr/include
	# Libs
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/*.{a,so*} $(1)/usr/lib
	# ISC Config Script
	$(INSTALL_DIR) $(1)/usr/bin
	$(CP) $(PKG_INSTALL_DIR)/usr/bin/*.sh $(1)/usr/bin
endef

define Package/bind-libs/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/*.so* $(1)/usr/lib
endef

define Package/bind-server/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/named $(1)/usr/sbin/
	$(INSTALL_DIR) $(1)/etc/bind
	$(CP) \
		./files/bind/db.0 \
		./files/bind/db.127 \
		./files/bind/db.255 \
		./files/bind/db.local \
		./files/bind/db.root \
		$(1)/etc/bind/
	$(CP) ./files/bind/named.conf.example $(1)/etc/bind/named.conf
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/named.init $(1)/etc/init.d/named
	find $(1)/etc/bind/ -name ".svn" | xargs rm -rf
endef

define Package/bind-server/conffiles
/etc/bind/db.0
/etc/bind/db.127
/etc/bind/db.255
/etc/bind/db.local
/etc/bind/db.root
/etc/bind/named.conf
endef

define Package/bind-client/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/nsupdate $(1)/usr/bin/
endef

define Package/bind-tools/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/dig $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/host $(1)/usr/bin/
	# Additional Tools '/usr/bin' - Changed by Mammoth
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/delv $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/nsupdate $(1)/usr/bin/
	#$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/mdig $(1)/usr/bin/
	#$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/named-rrchecker $(1)/usr/bin/
	#$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/arpaname $(1)/usr/bin/
	#$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/bind9-config $(1)/usr/bin/
	#$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/isc-config.sh $(1)/usr/bin/
	# Provided by busybox
	#$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/nslookup $(1)/usr/bin/
	# End Additional Tools '/usr/bin' - Changed by Mammoth
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/dnssec-keygen $(1)/usr/sbin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/dnssec-signzone $(1)/usr/sbin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/named-checkconf $(1)/usr/sbin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/named-checkzone $(1)/usr/sbin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/rndc $(1)/usr/sbin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/rndc-confgen $(1)/usr/sbin/
	# Additional Tools '/usr/sbin' - Changed by Mammoth
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/ddns-confgen $(1)/usr/sbin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/dnssec-dsfromkey $(1)/usr/sbin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/dnssec-importkey $(1)/usr/sbin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/dnssec-keyfromlabel $(1)/usr/sbin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/dnssec-revoke $(1)/usr/sbin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/dnssec-settime $(1)/usr/sbin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/dnssec-verify $(1)/usr/sbin/
	$(LN) named-checkzone $(1)/usr/sbin/named-compilezone
	$(LN) ddns-confgen $(1)/usr/sbin/tsig-keygen

endef

define Package/bind-rndc/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/rndc $(1)/usr/sbin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/rndc-confgen $(1)/usr/sbin/
endef

# Additional Tools - Package added by Mammoth
define Package/bind-ddns/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/ddns-confgen $(1)/usr/sbin/
	$(LN) ddns-confgen $(1)/usr/sbin/tsig-keygen
endef

# Additional Tools - Package added by Mammoth
define Package/bind-dnssec-python/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/bin/python/dnssec-checkds.py $(1)/usr/sbin/
	mv $(1)/usr/sbin/dnssec-checkds.py $(1)/usr/sbin/dnssec-checkds
	sed -i -e 's/#!/#!\/usr\/bin\/env python/g' $(1)/usr/sbin/dnssec-checkds
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/bin/python/dnssec-coverage.py $(1)/usr/sbin/
	mv $(1)/usr/sbin/dnssec-coverage.py $(1)/usr/sbin/dnssec-coverage
	sed -i -e 's/#!/#!\/usr\/bin\/env python/g' $(1)/usr/sbin/dnssec-coverage
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/bin/python/dnssec-keymgr.py $(1)/usr/sbin/
	mv $(1)/usr/sbin/dnssec-keymgr.py $(1)/usr/sbin/dnssec-keymgr
	sed -i -e 's/#!/#!\/usr\/bin\/env python/g' $(1)/usr/sbin/dnssec-keymgr

	$(INSTALL_DIR) $(1)$(PYTHON_PKG_DIR)/isc
	$(CP) $(PKG_BUILD_DIR)/bin/python/isc/*.py $(1)$(PYTHON_PKG_DIR)/isc
endef

define Package/bind-check/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/named-checkconf $(1)/usr/sbin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/named-checkzone $(1)/usr/sbin/
	# Additional Tools - Changed by Mammoth
	$(LN) named-checkzone $(1)/usr/sbin/named-compilezone
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/named-journalprint $(1)/usr/sbin/
endef

define Package/bind-dnssec/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/dnssec-keygen $(1)/usr/sbin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/dnssec-signzone $(1)/usr/sbin/
	# Additional Tools - Changed by Mammoth
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/dnssec-dsfromkey $(1)/usr/sbin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/dnssec-importkey $(1)/usr/sbin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/dnssec-keyfromlabel $(1)/usr/sbin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/dnssec-revoke $(1)/usr/sbin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/dnssec-settime $(1)/usr/sbin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/dnssec-verify $(1)/usr/sbin/
endef

define Package/bind-host/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/host $(1)/usr/bin/
endef

define Package/bind-dig/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/dig $(1)/usr/bin/
endef

$(eval $(call BuildPackage,bind-libs))
$(eval $(call BuildPackage,bind-server))
$(eval $(call BuildPackage,bind-client))
$(eval $(call BuildPackage,bind-tools))
$(eval $(call BuildPackage,bind-rndc))
$(eval $(call BuildPackage,bind-ddns))
$(eval $(call BuildPackage,bind-check))
$(eval $(call BuildPackage,bind-dnssec))
$(eval $(call BuildPackage,bind-dnssec-python))
$(eval $(call BuildPackage,bind-host))
$(eval $(call BuildPackage,bind-dig))
