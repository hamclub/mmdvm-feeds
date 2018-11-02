#
# Copyright (C) 2008-2011 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=p25-clients
#PKG_REV:=HEAD
PKG_REV:=6d59f8f
PKG_VERSION:=BG5HHP-20181004
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.bz2
PKG_SOURCE_URL:=https://github.com/g4klx/P25Clients
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_VERSION:=$(PKG_REV)
PKG_SOURCE_PROTO:=git

include $(INCLUDE_DIR)/package.mk

define Package/p25-clients
  SECTION:=net
  CATEGORY:=Network
  TITLE:=P25Clients
  URL:=https://github.com/g4klx/P25Clients/
  DEPENDS:=+libpthread +libstdcpp
endef

define Package/p25-clients/description
	The P25Clients package including P25Gateway,P25Parrot. 
	Build for OpenWrt, by BG5HHP
endef

TARGET_CFLAGS += -std=c++0x -DOPENWRT=1

TARGET_LDFLAGS += \
        -Wl,-rpath-link=$(STAGING_DIR)/usr/lib \

MAKE_FLAGS +=\
        CFLAGS="$(TARGET_CFLAGS) $(TARGET_CPPFLAGS)" \
        LDFLAGS="$(TARGET_LDFLAGS)" \

define Package/p25-clients/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/P25Gateway/P25Gateway $(1)/usr/sbin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/P25Parrot/P25Parrot $(1)/usr/sbin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/P25Reflector/P25Reflector $(1)/usr/sbin/
	$(CP) -a files/* $(1)/
endef

define Package/p25-clients/conffiles
/etc/P25Gateway.ini
/etc/P25Reflector.ini
endef

define Package/p25-clients/postinst
#!/bin/sh
if [ -e /etc/openwrt_release ]; then
	uci set mmdvm.@p25gateway[0].enabled=1 && uci commit mmdvm || :
	[ -f /etc/init.d/p25gateway ] && /etc/init.d/p25gateway enable || :
fi
exit 0
endef

$(eval $(call BuildPackage,p25-clients))
