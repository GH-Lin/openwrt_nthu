#
# Copyright (C) 2015 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=vpp
PKG_VERSION:=23.06-rc1

PKG_SOURCE:=v$(PKG_VERSION).tar.gz
PKG_SOURCE_URL=https://github.com/FDio/vpp/archive/refs/tags/
PKG_HASH:=064310548b9bcacb51b214122c46b0e89f5bf3248ec2ec1fc5d72e5829cd2875

PKG_LICENSE:=Apache-2.0

PKG_FORTIFY_SOURCE:=2


include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/cmake.mk

TARGET_CFLAGS += -I$(STAGING_DIR)/include
TARGET_LDFLAGS += -L$(STAGING_DIR)/usr/lib -L$(STAGING_DIR)/lib

define Package/vpp
  SECTION:=net
  CATEGORY:=Network
  TITLE:=Cisco's Vector Packet Processing (VPP) technology.
  DEPENDS:=+libarchive +libelf +libnuma +libopenssl +libpcap +libuuid \
           +jansson +USE_GLIBC:libbsd +libnl +libmnl
endef

define Build/Configure
	CFLAGS="$(TARGET_CPPFLAGS) $(TARGET_CFLAGS)" \
	$(MAKE) -C $(PKG_BUILD_DIR) \
		$(TARGET_CONFIGURE_OPTS) \
		install-dep -i
endef

define Build/Compile
	CFLAGS="$(TARGET_CPPFLAGS) $(TARGET_CFLAGS)" \
	$(MAKE) -C $(PKG_BUILD_DIR) \
		$(TARGET_CONFIGURE_OPTS) \
		LDFLAGS="$(TARGET_LDFLAGS)" build
endef

define Build/Install/Default
	$(NINJA) -C $(PKG_BUILD_DIR)/build-root/build-vpp_debug-native/vpp install
endef

define Package/vpp/install
	$(CP) $(PKG_BUILD_DIR)/build-root/install-vpp_debug-native/external/lib64/* \
		$(PKG_BUILD_DIR)/build-root/install-vpp_debug-native/external/lib
	$(INSTALL_DIR) $(1)/bin $(1)/etc $(1)/include
	$(INSTALL_DIR) $(1)/lib $(1)/libexec
	$(INSTALL_DIR) $(1)/sbin $(1)/share
	gzip -d $(TOPDIR)/package/network/services/vpp/dpdk_plugin.so.gz
	$(CP) $(PKG_BUILD_DIR)/build-root/install-vpp_debug-native/external/bin/* $(1)/bin
	$(CP) $(PKG_BUILD_DIR)/build-root/install-vpp_debug-native/external/etc/* $(1)/etc
	$(CP) $(PKG_BUILD_DIR)/build-root/install-vpp_debug-native/external/include/* $(1)/include
	$(CP) $(PKG_BUILD_DIR)/build-root/install-vpp_debug-native/external/lib/* $(1)/lib
	$(CP) $(PKG_BUILD_DIR)/build-root/install-vpp_debug-native/external/libexec/* $(1)/libexec
	$(CP) $(PKG_BUILD_DIR)/build-root/install-vpp_debug-native/external/sbin/* $(1)/sbin
	$(CP) $(PKG_BUILD_DIR)/build-root/install-vpp_debug-native/vpp/bin/* $(1)/bin
	$(CP) $(PKG_BUILD_DIR)/build-root/install-vpp_debug-native/vpp/etc/* $(1)/etc
	$(CP) $(PKG_BUILD_DIR)/build-root/install-vpp_debug-native/vpp/include/* $(1)/include
	$(CP) $(PKG_BUILD_DIR)/build-root/install-vpp_debug-native/vpp/lib/* $(1)/lib
	$(CP) $(PKG_BUILD_DIR)/build-root/install-vpp_debug-native/vpp/share/* $(1)/share
	$(CP) $(TOPDIR)/package/network/services/vpp/dpdk_plugin.so $(1)/lib/vpp_plugins
endef

$(eval $(call BuildPackage,vpp))
