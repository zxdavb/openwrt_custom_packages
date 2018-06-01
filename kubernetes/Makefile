#
# This software is licensed under the Public Domain.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=kubernetes
PKG_VERSION:=1.10.3
PKG_RELEASE:=1

#KG_SOURCE_PROTO:=git
#KG_SOURCE_URL:=https://github.com/kubernetes/kubernetes.git
#PKG_SOURCE:=$(PKG_NAME).tar.gz
#PKG_SOURCE_VERSION:=v1.3.0
#PKG_SOURCE_SUBDIR:=$(PKG_NAME)-temp
#PKG_HASH:=a630ff3b0dba9ec7f8dddf04019de0dc74adcff970c79074a8f75e7f4ed03db7
PKG_SOURCE:=v$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://github.com/kubernetes/kubernetes/archive/
PKG_HASH:=f3750b19d6d7471ea33230f96a420c53df46d8a060a4f14722992c5c7bdd9055

PKG_MAINTAINER:=Manfred Gschweidl <m.gschweidl@gmail.com>
PKG_LICENSE_FILES:=LICENSE

#PKG_INSTALL:=1
#PKG_BUILD_DEPENDS:=libprotobuf-c protobuf golang-github.com-google-gopacket
PKG_BUILD_DEPENDS:=go-bindata/host

# Default:
# PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)
# DUMP
#DUMP:=1
PKG_USE_GO:=1
PKG_GO_IMPORT_PATH:=k8s.io/kubernetes

include $(INCLUDE_DIR)/package.mk

#go_modules:=github.com/google/gopacket github.com/google/gopacket/layers \
#	github.com/google/gopacket/pcap github.com/google/gopacket/pfring \
#	github.com/google/gopacket/afpacket github.com/gopacket/tcpassembly

#go_modules:=github.com/google/gopacket github.com/google/gopacket/layers \
#	github.com/google/gopacket/pcap

#TAR_CMD:=$(HOST_TAR) -C $(PKG_BUILD_DIR) --strip-components 1 $(TAR_OPTIONS)

define Package/kubernetes/Default
	SUBMENU:=Golang
	SECTION:=lang
	CATEGORY:=Languages
	TITLE:=v$(PKG_VERSION)
	URL:=http://kubernetes.io/
	MAINTAINER:=Manfred Gschweidl <m.gschweidl@gmail.com>
	#DEPENDS:=+libpcap +libxml2 +golang-github.com-google-gopacket
	DEPENDS:=
	#DEFAULT:=n
endef

# Package descriptions
define Package/kubernetes/description/Default
 Kubernetes is an open source system for managing containerized applications across
multiple hosts; providing basic mechanisms for deployment, maintenance, and scaling
of applications.
endef

define Package/kubernetes-server/description
	$(call Package/kubernetes/description/Default)
	This is the server package, including node and client binaries as well.
endef

define Package/kubernetes-node/description
	$(call Package/kubernetes/description/Default)
	This is the node package, including client binaries as well.
endef

define Package/kubernetes-client/description
	$(call Package/kubernetes/description/Default)
	This is the client package.
endef

# Package definitions
define Package/kubernetes-server
	$(call Package/kubernetes/Default)
	TITLE+= Server
endef

define Package/kubernetes-node
	$(call Package/kubernetes/Default)
	TITLE+= Node
endef

define Package/kubernetes-client
	$(call Package/kubernetes/Default)
	TITLE+= Client
endef

# Preparing and building
#define Build/Prepare
#	$(call Build/GO/Prepare/Default)
#endef

define Build/Compile
	$(call copy_go_source)
	# KUBE_RELEASE_RUN_TESTS=n KUBE_FASTBUILD=true => make quick-release / make release-skip-tests
	(cd $(GO_PKG_COMPILE_PATH) && make generated_files)
	(cd $(GO_PKG_COMPILE_PATH) && KUBE_BUILD_PLATFORMS="$(GOOS)/$(GOARCH)" make)
	(cd $(GO_PKG_COMPILE_PATH) && GO_PKG_BIN_PATH="$(GO_PKG_BIN_PATH)" make install-gopath)
endef

# Dev Installation
define Build/InstallDev
	# $(call Build/GO/InstallDev/Default,$(1),none)
endef

# Package installations
define Package/kubernetes-server/install
	$(call Package/GO/Install/Default,$(1),apiextensions-apiserver cloud-controller-manager
		hyperkube kube-aggregator kube-apiserver kube-controller-manager kube-proxy
		kube-scheduler kubeadm kubectl kubelet mounter,none,none)
endef

define Package/kubernetes-node/install
	$(call Package/GO/Install/Default,$(1),kubeadm kubectl kubelet kube-proxy,none,none)
endef

define Package/kubernetes-client/install
	$(call Package/GO/Install/Default,$(1),kubectl,none,none)
endef

$(eval $(call BuildPackage,kubernetes-server))
$(eval $(call BuildPackage,kubernetes-node))
$(eval $(call BuildPackage,kubernetes-client))
