# 
# Copyright (C) 2006 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#
# $Id$

include $(TOPDIR)/rules.mk

PKG_NAME:=iproute2
PKG_VERSION:=2.6.15-060110
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=http://developer.osdl.org/dev/iproute2/download/
PKG_MD5SUM:=04f57a6d366d36426d276178b600f5c5

include $(INCLUDE_DIR)/package.mk

define Package/iproute2/Default
  SECTION:=net
  CATEGORY:=Network
  URL:=http://linux-net.osdl.org/index.php/Iproute2
endef

define Package/ip
  $(call Package/iproute2/Default)
  TITLE:=Routing control utility
endef

define Package/tc
  $(call Package/iproute2/Default)
  TITLE:=Traffic control utility
endef

define Build/Configure
	$(SED) "s:-O2:${TARGET_CFLAGS}:g" $(PKG_BUILD_DIR)/Makefile
	$(SED) "s,-I/usr/include/db3,," $(PKG_BUILD_DIR)/Makefile
	$(SED) "s,^KERNEL_INCLUDE.*,KERNEL_INCLUDE=$(LINUX_DIR)/include," \
		$(PKG_BUILD_DIR)/Makefile
	$(SED) "s,^LIBC_INCLUDE.*,LIBC_INCLUDE=$(STAGING_DIR)/include," \
		$(PKG_BUILD_DIR)/Makefile
	# For now disable compiling of the misc directory because it seems to fail
	rm -rf $(PKG_BUILD_DIR)/misc 
	$(SED) "s, misc,," $(PKG_BUILD_DIR)/Makefile
endef

define Build/Compile
	$(MAKE) -C $(PKG_BUILD_DIR)/netem HOSTCC="$(HOSTCC)" CFLAGS="-D_GNU_SOURCE -O2 -Wstrict-prototypes -Wall -I ../include -DRESOLVE_HOSTNAMES"
	$(MAKE) -C $(PKG_BUILD_DIR) $(TARGET_CONFIGURE_OPTS) KERNEL_INCLUDE=$(LINUX_DIR)/include all tc/tc ip/ip
endef

define Package/ip/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/ip/ip $(1)/usr/sbin/
endef

define Package/tc/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/tc/tc $(1)/usr/sbin/
endef

$(eval $(call BuildPackage,ip))
$(eval $(call BuildPackage,tc))

$(eval $(call RequireCommand,bison, \
	$(PKG_NAME) requires GNU bison. \
))
