# 
# Copyright (C) 2006 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#
include $(TOPDIR)/rules.mk

ARCH:=mipsel
BOARD:=rb532
BOARDNAME:=Mikrotik RouterBoard 532
FEATURES:=jffs2 pci tgz

LINUX_VERSION:=2.6.21.5

include $(INCLUDE_DIR)/kernel-build.mk

# include the profiles
-include profiles/*.mk

$(eval $(call BuildKernel))
