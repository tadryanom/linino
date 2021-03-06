# $Id: Makefile 1146 2005-06-05 13:32:28Z nbd $

include $(TOPDIR)/rules.mk

PKG_NAME:=radiusclient-ng
PKG_VERSION:=0.5.2
PKG_RELEASE:=1
PKG_MD5SUM:=c54eb70e964bdd22dd44b39a9b4df8ca

PKG_SOURCE_URL:=http://download.berlios.de/radiusclient-ng/
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_CAT:=zcat

PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)
PKG_INSTALL_DIR:=$(PKG_BUILD_DIR)/ipkg-install

include $(TOPDIR)/package/rules.mk

$(eval $(call PKG_template,RADIUSCLIENT_NG,radiusclient-ng,$(PKG_VERSION)-$(PKG_RELEASE),$(ARCH)))
$(eval $(call PKG_template,LIBRADIUSCLIENT_NG,libradiusclient-ng,$(PKG_VERSION)-$(PKG_RELEASE),$(ARCH)))

$(PKG_BUILD_DIR)/.configured:
	(cd $(PKG_BUILD_DIR); rm -rf config.cache; \
		$(TARGET_CONFIGURE_OPTS) \
		CFLAGS="$(TARGET_CFLAGS)" \
		CPPFLAGS="-I$(STAGING_DIR)/usr/include -I$(STAGING_DIR)/include" \
		LDFLAGS="-L$(STAGING_DIR)/usr/lib -L$(STAGING_DIR)/lib" \
		ac_cv_func_getdomainname=yes \
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
			$(DISABLE_NLS) \
			$(DISABLE_LARGEFILE) \
	);
	touch $@

$(PKG_BUILD_DIR)/.built:
	$(MAKE) -C $(PKG_BUILD_DIR) \
		DESTDIR=$(PKG_INSTALL_DIR) \
		install-exec install-data
	touch $@

$(IPKG_RADIUSCLIENT_NG):
	install -d -m0755 $(IDIR_RADIUSCLIENT_NG)/etc
	cp -fpR $(PKG_INSTALL_DIR)/etc/radiusclient-ng $(IDIR_RADIUSCLIENT_NG)/etc/
	install -d -m0755 $(IDIR_RADIUSCLIENT_NG)/usr/sbin
	cp -fpR $(PKG_INSTALL_DIR)/usr/sbin/* $(IDIR_RADIUSCLIENT_NG)/usr/sbin/
	$(RSTRIP) $(IDIR_RADIUSCLIENT_NG)/
	$(IPKG_BUILD) $(IDIR_RADIUSCLIENT_NG) $(PACKAGE_DIR)

$(IPKG_LIBRADIUSCLIENT_NG):
	install -d -m0755 $(IDIR_LIBRADIUSCLIENT_NG)/usr/lib
	cp -fpR $(PKG_INSTALL_DIR)/usr/lib/libradiusclient-ng.so.* $(IDIR_LIBRADIUSCLIENT_NG)/usr/lib/
	$(RSTRIP) $(IDIR_LIBRADIUSCLIENT_NG)/
	$(IPKG_BUILD) $(IDIR_LIBRADIUSCLIENT_NG) $(PACKAGE_DIR)

$(STAGING_DIR)/usr/lib/libradiusclient-ng.so: $(PKG_BUILD_DIR)/.built
	mkdir -p $(STAGING_DIR)/usr/include
	cp -fpR $(PKG_INSTALL_DIR)/usr/include/radiusclient-ng.h $(STAGING_DIR)/usr/include/
	mkdir -p $(STAGING_DIR)/usr/lib/
	cp -fpR $(PKG_INSTALL_DIR)/usr/lib/libradiusclient-ng.{a,so*} $(STAGING_DIR)/usr/lib/
	touch $@

install-dev: $(STAGING_DIR)/usr/lib/libradiusclient-ng.so

uninstall-dev:
	rm -rf \
	  $(STAGING_DIR)/usr/include/radiusclient-ng.h \
	  $(STAGING_DIR)/usr/lib/libradiusclient-ng.{a,so*} \

compile: install-dev
clean: uninstall-dev

mostlyclean:
	-$(MAKE) -C $(PKG_BUILD_DIR) clean
	rm -f $(PKG_BUILD_DIR)/.built
