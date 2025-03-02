include $(TOPDIR)/rules.mk

PKG_NAME:=nyn
PKG_VERSION:=0.0.1
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/diredocks/nyn.git
PKG_SOURCE_VERSION:=HEAD         
PKG_MIRROR_HASH:=skip            

PKG_MAINTAINER:=Diredocks <napking@163.com>
PKG_LICENSE:=MIT                 
PKG_LICENSE_FILES:=LICENSE


PKG_BUILD_DEPENDS:=golang/host
PKG_BUILD_PARALLEL:=1
PKG_USE_MIPS16:=0


GO_PKG:=github.com/diredocks/nyn
GO_PKG_BUILD_PKG:=github.com/diredocks/nyn/cmd/nyn/main.go
GO_PKG_LDFLAGS:=-s -w


CGO_ENABLED:=1
PKG_BUILD_DEPENDS+= +libopenssl +libpcap

include $(INCLUDE_DIR)/package.mk
include $(TOPDIR)/feeds/packages/lang/golang/golang-package.mk

define Package/nyn
  SECTION:=net
  CATEGORY:=Network
  TITLE:=I am Not Your Node
  URL:=https://github.com/diredocks/nyn
  DEPENDS:=+libopenssl +libpcap $(GO_ARCH_DEPENDS)
endef

define Package/nyn/description
  Modern 802.1x standard authentication client with CGO support.
endef


define Build/Prepare
    $(call Build/Prepare/Default)
    (cd $(PKG_BUILD_DIR) && \
        export GOPATH="$(GOPATH)" && \
        export GO111MODULE=on && \
        go mod tidy && \
        go mod vendor)
endef


define Build/Configure
    (cd $(PKG_BUILD_DIR) && \
        export CGO_CFLAGS="$(TARGET_CFLAGS)" && \
        export CGO_LDFLAGS="$(TARGET_LDFLAGS)" && \
        export CC="$(TARGET_CC)" && \
        export GOOS=linux && \
        export GOARCH=$(GOARCH) && \
        export CGO_ENABLED=1
endef


define Build/Compile
    (cd $(PKG_BUILD_DIR) && \
        $(call GoPackage/Build/Compile) && \
        $(STAGING_DIR_HOST)/bin/upx --lzma --best $(PKG_BUILD_DIR)/bin/nyn || true)
endef


define Package/nyn/install
    $(INSTALL_DIR) $(1)/usr/bin
    $(INSTALL_BIN) $(PKG_BUILD_DIR)/bin/nyn $(1)/usr/bin/
endef

$(eval $(call GoBinPackage,nyn))
$(eval $(call BuildPackage,nyn))