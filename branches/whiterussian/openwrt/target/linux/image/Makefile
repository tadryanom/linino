include $(TOPDIR)/rules.mk

KDIR:=$(BUILD_DIR)/linux-$(KERNEL)-$(BOARD)

ifeq ($(BR2_TARGET_ROOTFS_JFFS2),y)
include ./jffs2.mk
endif

ifeq ($(BR2_TARGET_ROOTFS_SQUASHFS_LZMA),y)
include ./squashfs.mk
endif

prepare:
compile:
install:
	$(MAKE) -C $(BOARD) prepare
	$(MAKE) -C $(BOARD) compile

install-ib:
	-$(MAKE) -C $(BOARD) IB_DIR="$(IB_DIR)" install-ib
	mkdir -p $(IB_DIR)/build_$(ARCH)/linux-$(KERNEL)-$(BOARD)
	cp $(BUILD_DIR)/linux-$(KERNEL)-$(BOARD)/kernel[-_]*.ipk $(IB_DIR)/build_$(ARCH)/linux-$(KERNEL)-$(BOARD)/	

install-prepare:
	find $(KDIR)/root -type f -not -perm +0100 -not -name 'ssh_host*' | xargs chmod 0644
	find $(KDIR)/root -type f -perm +0100 | xargs chmod 0755
	find $(KDIR)/root -type d | xargs chmod 0755
	chmod 0777 $(KDIR)/root/tmp

rebuild: clean prepare compile install
clean:

