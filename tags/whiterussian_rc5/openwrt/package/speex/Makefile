# $Id$

include $(TOPDIR)/rules.mk

PKG_NAME:=speex
PKG_VERSION:=1.1.8
PKG_RELEASE:=1
PKG_MD5SUM:=734892c8ca5e7e835828048a1289a0cc

PKG_SOURCE_URL:=http://us.speex.org/download 
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_CAT:=zcat

PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)
PKG_INSTALL_DIR:=$(PKG_BUILD_DIR)/ipkg-install

include $(TOPDIR)/package/rules.mk

$(eval $(call PKG_template,LIBSPEEX,libspeex,$(PKG_VERSION)-$(PKG_RELEASE),$(ARCH)))

$(PKG_BUILD_DIR)/.configured: $(PKG_BUILD_DIR)/.prepared
	(cd $(PKG_BUILD_DIR); rm -rf config.{cache,status} ; \
		$(TARGET_CONFIGURE_OPTS) \
		CFLAGS="$(strip $(TARGET_CFLAGS)) -I$(STAGING_DIR)/include -I$(STAGING_DIR)/usr/include" \
		CPPFLAGS="-I$(STAGING_DIR)/usr/include" \
		LDFLAGS="-L$(STAGING_DIR)/usr/lib" \
		OGG_CFLAGS="" \
		./configure \
		--target=$(GNU_TARGET_NAME) \
		--host=$(GNU_TARGET_NAME) \
		--build=$(GNU_HOST_NAME) \
		  --program-prefix="" \
		  --program-suffix="" \
		  --prefix=/usr \
		  --exec-prefix=/usr \
		  --bindir=/usr/bin \
		  --datadir=/usr/share \
		  --includedir=/usr/include \
		  --infodir=/usr/share/info \
		  --libdir=/usr/lib \
		  --libexecdir=/usr/lib \
		  --localstatedir=/var \
		  --mandir=/usr/share/man \
		  --sbindir=/usr/sbin \
		  --sysconfdir=/etc \
		  $(DISABLE_LARGEFILE) \
		  $(DISABLE_NLS) \
		  --enable-shared \
		  --enable-static \
		  --disable-oggtest \
	);
	touch $@

$(PKG_BUILD_DIR)/.built:
	rm -rf $(PKG_INSTALL_DIR)
	mkdir -p $(PKG_INSTALL_DIR)
	$(MAKE) -C $(PKG_BUILD_DIR) \
		DESTDIR="$(PKG_INSTALL_DIR)" \
		OGG_CFLAGS="" \
	  	bin_PROGRAMS="" \
		all install
	touch $@

$(IPKG_LIBSPEEX):
	install -d -m0755 $(IDIR_LIBSPEEX)/usr/lib
	cp -fpR $(PKG_INSTALL_DIR)/usr/lib/libspeex.so.* $(IDIR_LIBSPEEX)/usr/lib/
	$(RSTRIP) $(IDIR_LIBSPEEX)
	$(IPKG_BUILD) $(IDIR_LIBSPEEX) $(PACKAGE_DIR)

$(STAGING_DIR)/usr/lib/libspeex.so: $(PKG_BUILD_DIR)/.built
	mkdir -p $(STAGING_DIR)/usr/include
	cp -fpR $(PKG_INSTALL_DIR)/usr/include/speex $(STAGING_DIR)/usr/include/
	mkdir -p $(STAGING_DIR)/usr/lib
	cp -fpR $(PKG_INSTALL_DIR)/usr/lib/libspeex.{a,so*} $(STAGING_DIR)/usr/lib/
	mkdir -p $(STAGING_DIR)/usr/lib/pkgconfig
	cp -fpR $(PKG_INSTALL_DIR)/usr/lib/pkgconfig/speex.pc $(STAGING_DIR)/usr/lib/pkgconfig/
	touch $@

install-dev: $(STAGING_DIR)/usr/lib/libspeex.so

uninstall-dev:
	rm -rf \
		$(STAGING_DIR)/usr/include/speex \
		$(STAGING_DIR)/usr/lib/libspeex.{a,so*} \
		$(STAGING_DIR)/usr/lib/pkgconfig/speex.pc \

compile: install-dev
clean: uninstall-dev
