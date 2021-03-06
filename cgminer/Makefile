#
# Author: Xiangfu Liu <xiangfu@openmobilefree.net>
# Address: 12h6gdGnThW385JaX1LRMA8cXKmbYRTP8Q
#
# This is free and unencumbered software released into the public domain.
# For details see the UNLICENSE file at the root of the source tree.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=cgminer

PKG_VERSION:=4.7.1.hfr
PKG_REV:=hfr

PKG_RELEASE:=1
PKG_INSTALL:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION)-$(PKG_REV).tar.bz2
PKG_SOURCE_URL:=git://github.com/binarier/cgminer.git
PKG_SOURCE_PROTO:=git
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_VERSION:=$(PKG_REV)

PKG_FIXUP:=autoreconf

include $(INCLUDE_DIR)/package.mk

define Package/cgminer
	SECTION:=utils
	CATEGORY:=Utilities
	TITLE:=cgminer
	URL:=https://github.com/binarier/cgminer
	DEPENDS:=+libcurl +libpthread +jansson +udev +libncurses
endef

define Package/cgminer/description
Cgminer is a multi-threaded multi-pool GPU, FPGA and CPU miner with ATI GPU
monitoring, (over)clocking and fanspeed support for bitcoin and derivative
coins. Do not use on multiple block chains at the same time!
endef

define Package/cgminer/config
	menu "Configuration"
	depends on PACKAGE_cgminer
	source "$(SOURCE)/Config.in"
	endmenu
endef

CONFIGURE_ARGS += --enable-hfr
TARGET_LDFLAGS += -Wl,-rpath-link=$(STAGING_DIR)/usr/lib

define Build/Compile
	$(call Build/Compile/Default)
	(cd $(PKG_BUILD_DIR) && \
	  $(TARGET_CC) -Icompat/jansson \
	  api-example.c -o cgminer-api;)
endef

define Package/cgminer/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_DIR) $(1)/etc/config

	$(INSTALL_BIN) $(PKG_BUILD_DIR)/cgminer-api	   $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/cgminer  $(1)/usr/bin
	$(INSTALL_BIN) $(FILES_DIR)/cgminer-monitor        $(1)/usr/bin

	$(INSTALL_BIN) $(FILES_DIR)/cgminer.avalon2.init   $(1)/etc/init.d/cgminer
	$(CP)          $(FILES_DIR)/cgminer.avalon2.config $(1)/etc/config/cgminer
endef

$(eval $(call BuildPackage,cgminer))
