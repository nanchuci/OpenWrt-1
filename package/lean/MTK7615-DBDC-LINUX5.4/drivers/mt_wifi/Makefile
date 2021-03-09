#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/kernel.mk

PKG_NAME:=mt_wifi
P4REV:=8
PKG_VERSION:=1
PKG_RELEASE:=1

PKG_CONFIG_DEPENDS := \
	CONFIG_MTK_CHIP_MT7603E_MT7612E \
	CONFIG_MTK_CHIP_MT7603E_MT7615E \
	CONFIG_MTK_CHIP_MT7615E \
	CONFIG_MTK_CHIP_MT7615E_DBDC

include $(INCLUDE_DIR)/package.mk

TAR_CMD=$(HOST_TAR) -C $(1)/ $(TAR_OPTIONS)

define KernelPackage/mt_wifi
  CATEGORY:=Kernel modules
  TITLE:=MTK wifi AP driver
  DEPENDS:=@TARGET_ramips +wireless-tools
  CONFLICTS:=kmod-mt7615d_dbdc
  SUBMENU:=Wireless
endef

define KernelPackage/mt_wifi/config
	source "$(SOURCE)/config.in"
endef

define KernelPackage/mt_wifi/description
  MT_WiFi auto config for mt7603/mt7612/mt7615 chips
endef

define Build/Compile
endef

define KernelPackage/mt_wifi/install
	$(INSTALL_DIR) $(1)/etc/init.d/
	$(INSTALL_DIR) $(1)/lib/wifi/
	$(INSTALL_DIR) $(1)/etc/wireless/mt7615/
	$(INSTALL_DIR) $(1)/etc_ro/Wireless/RT2860AP/
	$(INSTALL_DIR) $(1)/lib/preinit/
ifeq ($(CONFIG_MTK_CHIP_MT7603E_MT7612E),y)
	$(INSTALL_BIN) ./files/7603_7612-l1profile.dat $(1)/etc/wireless/l1profile.dat
	$(INSTALL_BIN) ./files/mt7603.dat $(1)/etc/wireless/mt7615/mt7615.1.dat
	$(INSTALL_BIN) ./files/mt7612.dat $(1)/etc/wireless/mt7615/mt7615.2.dat
else ifeq ($(CONFIG_MTK_CHIP_MT7603E_MT7615E),y)
	$(INSTALL_BIN) ./files/7603_7615-l1profile.dat $(1)/etc/wireless/l1profile.dat
	$(INSTALL_BIN) ./files/mt7603.dat $(1)/etc/wireless/mt7615/mt7615.1.dat
	$(INSTALL_BIN) ./files/mt7615.dat $(1)/etc/wireless/mt7615/mt7615.2.dat
else
ifeq ($(CONFIG_MTK_CHIP_MT7615E_DBDC),y)
	$(INSTALL_BIN) ./files/dbdc.l1profile.dat $(1)/etc/wireless/l1profile.dat
	$(INSTALL_BIN) ./files/mt7615.1.2G.dat $(1)/etc/wireless/mt7615/
	$(INSTALL_BIN) ./files/mt7615.1.5G.dat $(1)/etc/wireless/mt7615/
else
	$(INSTALL_BIN) ./files/7615.l1profile.dat $(1)/etc/wireless/l1profile.dat
	$(INSTALL_BIN) ./files/mt7615.2G.dat $(1)/etc/wireless/mt7615/mt7615.1.dat
	$(INSTALL_BIN) ./files/mt7615.5G.dat $(1)/etc/wireless/mt7615/mt7615.2.dat
endif
endif
	$(INSTALL_BIN) ./files/SingleSKU.dat $(1)/etc_ro/Wireless/RT2860AP/
	$(INSTALL_BIN) ./files/SingleSKU_BF.dat $(1)/etc_ro/Wireless/RT2860AP/
	$(INSTALL_BIN) ./files/mt7615.lua $(1)/lib/wifi
	$(INSTALL_BIN) ./files/firmware.sh $(1)/etc/init.d/
	$(INSTALL_BIN) ./files/82_load_wifi.sh $(1)/lib/preinit/
endef

$(eval $(call KernelPackage,mt_wifi))
