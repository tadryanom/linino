#
# Copyright (C) 2009 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#
include $(TOPDIR)/rules.mk

ARCH:=mipsel
BOARD:=cobalt
BOARDNAME:=Cobalt Microservers
FEATURES:=tgz pci ext2
CFLAGS:=-O2 -pipe -mtune=r5000 -funit-at-a-time

LINUX_VERSION:=2.6.32.10

include $(INCLUDE_DIR)/target.mk

define Target/Description
	Build firmware images for Cobalt servers systems (Qube/Qube2/RaQ/RaQ2).
endef

$(eval $(call BuildTarget))
