# SPDX-License-Identifier: GPL-2.0-only
#
# Copyright (C) 2006-2011 OpenWrt.org

include $(TOPDIR)/rules.mk

ARCH:=i386
BOARD:=x86
BOARDNAME:=x86
FEATURES:=squashfs ext4 vdi vmdk vhdx pcmcia targz fpu boot-part rootfs-part
SUBTARGETS:=generic legacy geode 64

KERNEL_PATCHVER:=6.12
KERNEL_TESTING_PATCHVER:=6.6

KERNELNAME:=bzImage

include $(INCLUDE_DIR)/target.mk

DEFAULT_PACKAGES += \
	partx-utils mkf2fs \
	e2fsprogs kmod-button-hotplug \
	grub2-bios-setup \
	fdisk kmod-usb-hid kmod-ath5k kmod-ath9k kmod-ath9k-htc kmod-ath10k kmod-rt2800-usb kmod-e1000e kmod-igb kmod-igbvf kmod-igc kmod-ixgbe kmod-pcnet32 kmod-tulip kmod-vmxnet3 kmod-i40e kmod-i40evf kmod-r8125 kmod-8139cp kmod-8139too kmod-fs-f2fs \
	lm-sensors autocore-x86 \
	ath10k-firmware-qca988x ath10k-firmware-qca9888 ath10k-firmware-qca9984 brcmfmac-firmware-43602a1-pcie \
	alsa-utils kmod-ac97 kmod-sound-hda-core kmod-sound-hda-codec-realtek kmod-sound-hda-codec-via kmod-sound-via82xx kmod-usb-audio \
	kmod-usb-net kmod-usb-net-asix kmod-usb-net-asix-ax88179 kmod-usb-net-ipheth kmod-usb-net-rndis kmod-usb-net-rtl8150 kmod-usb-net-rtl8152 \
	luci-app-usb-printer luci-app-vsftpd

$(eval $(call BuildTarget))

$(eval $(call $(if $(CONFIG_ISO_IMAGES),SetupHostCommand,Ignore),mkisofs, \
	Please install mkisofs. , \
	mkisofs -v 2>&1 , \
	genisoimage -v 2>&1 | grep genisoimage, \
	xorrisofs -v 2>&1 | grep xorriso \
))
