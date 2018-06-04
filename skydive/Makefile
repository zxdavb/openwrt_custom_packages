#
# This software is licensed under the Public Domain.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=skydive
PKG_VERSION:=0.17.0
PKG_RELEASE:=1

PKG_SOURCE_PROTO=git
PKG_SOURCE_URL:=https://github.com/skydive-project/skydive.git
PKG_SOURCE_VERSION:=v$(PKG_VERSION)
PKG_SOURCE_DATE:=2018-05-31
PKG_MIRROR_HASH:=fe01b6d3862d9dd06ce55ac5331cd4e4b37b4ef985cbd8a73e7fd481842e64cf

PKG_MAINTAINER:=Manfred Gschweidl <m.gschweidl@gmail.com>
PKG_LICENSE_FILES:=LICENSE

PKG_BUILD_DEPENDS:=protobuf protobuf-c govendor/host

PKG_USE_GO:=1
PKG_GO_IMPORT_PATH:=github.com/skydive-project/skydive

include $(INCLUDE_DIR)/package.mk

define Package/skydive
	SUBMENU:=Golang
	SECTION:=lang
	CATEGORY:=Languages
	TITLE:=v$(PKG_VERSION) - Open source network analyzer
	URL:=https://skydive.network
	MAINTAINER:=Manfred Gschweidl <m.gschweidl@gmail.com>
	DEPENDS:=+libpcap +libxml2 +golang-github.com-google-gopacket
endef

define Package/skydive/description
 An open source real-time network topology and protocols analyze

 Skydive is an open source real-time network topology and protocols
 analyzer. It aims to provide a comprehensive way of understanding
 what is happening in the network infrastructure.

 Skydive agents collect topology informations and flows and forward
 them to a central agent for further analysis. All the informations
 are stored in an Elasticsearch database.

 Skydive is SDN-agnostic but provides SDN drivers in order to enhance
 the topology and flows informations.
endef

export PATH=$(GOPATH)/bin:$(GOPATH)/$(GO_PKG_BIN_PATH):$(TARGET_PATH_PKG)
define Build/Compile
	$(call copy_go_source)
#	( cd $(GO_PKG_COMPILE_PATH) && PATH=$(PATH) WITH_DPDK=true make install )
	( cd $(GO_PKG_COMPILE_PATH) && PATH=$(PATH) make install )
endef

define Package/skydive/install
	$(call Package/GO/Install/Default,$(1),skydive,none,none)
endef

$(eval $(call BuildPackage,skydive))
