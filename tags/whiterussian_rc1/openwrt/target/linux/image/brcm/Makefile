include $(TOPDIR)/rules.mk

KDIR:=$(BUILD_DIR)/linux-$(KERNEL)-brcm

lzma-loader-clean:
	$(MAKE) -C lzma-loader clean

lzma-loader-prepare:
	$(MAKE) -C lzma-loader prepare

lzma-loader-compile: lzma-loader-prepare
	$(MAKE) -C lzma-loader compile

$(KDIR)/vmlinux.lzma: $(KDIR)/vmlinux
	cat $^ | $(STAGING_DIR)/bin/lzma e -si -so -eos -lc1 -lp2 -pb2 > $@ || (rm -f $@ && false)

ifeq ($(FS),jffs2-8MB)
TRXALIGN:=-a 0x20000
endif
ifeq ($(FS),jffs2-4MB)
TRXALIGN:=-a 0x10000
endif
$(BIN_DIR)/openwrt-brcm-$(KERNEL)-$(FS).trx: $(KDIR)/vmlinux.lzma
	$(STAGING_DIR)/bin/trx -o $@ $(BUILD_DIR)/loader.gz $(KDIR)/vmlinux.lzma $(TRXALIGN) $(KDIR)/root.$(FS)


ifeq ($(KERNEL),2.4)
FSNAME:=$(patsubst jffs2-%,jffs2,$(FS))

ifneq ($(FS),jffs2-8MB)
$(BIN_DIR)/openwrt-wrt54g-$(FSNAME).bin: $(BIN_DIR)/openwrt-brcm-$(KERNEL)-$(FS).trx
	$(STAGING_DIR)/bin/addpattern -2 -i $< -o $@ -g
	$(SED) "1s,^W54S,W54G," $@

install: $(BIN_DIR)/openwrt-wrt54g-$(FSNAME).bin
endif

ifneq ($(FS),jffs2-4MB)
$(BIN_DIR)/openwrt-wrt54gs-$(FSNAME).bin: $(BIN_DIR)/openwrt-brcm-$(KERNEL)-$(FS).trx
	$(STAGING_DIR)/bin/addpattern -2 -i $< -o $@ -g

install: $(BIN_DIR)/openwrt-wrt54gs-$(FSNAME).bin
endif

$(BIN_DIR)/openwrt-motorola-$(FS).bin: $(BIN_DIR)/openwrt-brcm-$(KERNEL)-$(FS).trx
	$(STAGING_DIR)/bin/motorola-bin $< $@
install: $(BIN_DIR)/openwrt-motorola-$(FS).bin
endif

clean: lzma-loader-clean
prepare: lzma-loader-prepare
compile: lzma-loader-compile
install: compile $(BIN_DIR)/openwrt-brcm-$(KERNEL)-$(FS).trx
	
