#
# Copyright (C) 2017,2018 BG5HHP
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=mmdvm-host
#PKG_REV:=HEAD
PKG_REV:=3f65200
PKG_VERSION:=BG5HHP-20210627
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.bz2
PKG_SOURCE_URL:=https://github.com/g4klx/MMDVMHost
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_VERSION:=$(PKG_REV)
PKG_SOURCE_PROTO:=git

PKG_MIRROR_HASH:=skip

include $(INCLUDE_DIR)/package.mk

define Package/mmdvm-host
  SECTION:=net
  CATEGORY:=Network
  TITLE:=MMDVM Host
  URL:=https://github.com/g4klx/MMDVMHost/
  DEPENDS:=+libpthread +libstdcpp
endef

define Package/mmdvm-host/description
	The MMDVMHost package for OpenWrt, by BG5HHP
endef

TARGET_CFLAGS += -std=c++0x -DOPENWRT=1

ifeq ($(ARCH),mips)
	TARGET_CFLAGS += -DWORDS_BIGENDIAN=1
	TARGET_CPPFLAGS += -DWORDS_BIGENDIAN=1
endif

TARGET_LDFLAGS += \
        -Wl,-rpath-link=$(STAGING_DIR)/usr/lib \

MAKE_FLAGS +=\
        CFLAGS="$(TARGET_CFLAGS) $(TARGET_CPPFLAGS)" \
        LDFLAGS="$(TARGET_LDFLAGS)" \

define Package/mmdvm-host/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/MMDVMHost $(1)/usr/sbin/
	$(CP) -a files/* $(1)/
endef

define Package/mmdvm-host/conffiles
/etc/MMDVM.ini
endef

define Package/mmdvm-host/postinst
#!/bin/sh
if [ -e /etc/openwrt_release ]; then
	uci set mmdvm.@mmdvmhost[0].enabled=1 && uci commit mmdvm || :
	[ -f /etc/init.d/mmdvmhost ] && /etc/init.d/mmdvmhost enable || :
fi
exit 0
endef

$(eval $(call BuildPackage,mmdvm-host))
