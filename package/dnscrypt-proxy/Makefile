include $(TOPDIR)/rules.mk

PKG_NAME:=dnscrypt-proxy
PKG_VERSION:=1.4.0
PKG_RELEASE:=5.E

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.bz2
PKG_SOURCE_URL:=http://download.dnscrypt.org/dnscrypt-proxy
PKG_MD5SUM:=40b5b73f5042330b86084460d7c839c6

PKG_INSTALL:=1
PKG_FIXUP:=autoreconf

include $(INCLUDE_DIR)/package.mk

define Package/dnscrypt-proxy/Default
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=IP Addresses and Names
  DEPENDS:=+libsodium
  URL:=http://dnscrypt.org/
  MAINTAINER:=Black Roland https://github.com/black-roland/exOpenWrt
endef

define Package/dnscrypt-proxy
  $(call Package/dnscrypt-proxy/Default)
  TITLE:=A tool for securing communications between a client and a DNS resolver
endef

define Package/dnscrypt-proxy/description
	dnscrypt-proxy provides local service which can be used directly as your
	local resolver or as a DNS forwarder, encrypting and authenticating requests
	using the DNSCrypt protocol and passing them to an upstream server.
	The DNSCrypt protocol uses high-speed high-security elliptic-curve cryptography
	and is very similar to DNSCurve, but focuses on securing communications between
	a client and its first-level resolver.
endef

define Package/hostip
  $(call Package/dnscrypt-proxy/Default)
  TITLE:=Resolver to IPv4 or IPv6 addresses
endef

define Package/hostip/description
	The DNSCrypt proxy ships with a simple tool named hostip that resolves a name
	to IPv4 or IPv6 addresses.
endef

define Build/Configure
	$(call Build/Configure/Default, \
		--prefix=/usr \
		--disable-ssp \
	)
endef

TARGET_CFLAGS += \
	-fomit-frame-pointer \
	-fdata-sections \
	-ffunction-sections

TARGET_LDFLAGS += \
	-Wl,-gc-sections

MAKE_FLAGS += \
	CFLAGS="$(TARGET_CFLAGS)" \
	LDFLAGS="$(TARGET_LDFLAGS)"

define Package/dnscrypt-proxy/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(CP) $(PKG_INSTALL_DIR)/usr/sbin/dnscrypt-proxy $(1)/usr/sbin/
	$(INSTALL_DIR) $(1)/usr/share/dnscrypt-proxy
	$(CP) $(PKG_INSTALL_DIR)/usr/share/dnscrypt-proxy/dnscrypt-resolvers.csv $(1)/usr/share/dnscrypt-proxy/
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/dnscrypt-proxy.init $(1)/etc/init.d/dnscrypt-proxy
	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_CONF) ./files/dnscrypt-proxy.config $(1)/etc/config/dnscrypt-proxy
endef

define Package/dnscrypt-proxy/conffiles
/etc/config/dnscrypt-proxy
endef

define Package/hostip/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(CP) $(PKG_INSTALL_DIR)/usr/bin/hostip $(1)/usr/bin/
endef

$(eval $(call BuildPackage,dnscrypt-proxy))
$(eval $(call BuildPackage,hostip))

