# Moved: See https://codeberg.org/ced4rtree/plymouth-openrc

# Plymouth for openrc

## This contains multiple openrc scripts to start plymouth and boot into the display manager of your choice

### Dependencies

- A display manager that is supported (listed in one of the files)

- Plymouth

- OpenRC

### Installation

Add the following:

```
# in /etc/default/grub
GRUB_CMDLINE_LINUX_DEFAULT="... quiet splash" # Replace ... with whatever is already there by default
```

```
# in /etc/mkinitcpio.conf
HOOKS=(... plymouth ...)  # Replace ... with whatever you already have
```
Issue the following in a terminal

```
# Use sudo if you do not have doas

doas grub-mkconfig -o /boot/grub/grub.cfg # rebuild the grub config since we changed it

doas mkinitcpio -P # rebuild the initramfs since we changed it

doas cp ${display_manager_of_your_choice} /etc/init.d/

doas cp plymouth-quit /etc/init.d/

doas rc-update add ${display_manager_of_your_choice} default # make sure we start the display manager (with some extra touches for plymouth)

doas rc-update add plymouth-quit default # boots into your display manager without just hanging on plymouth forever

doas cp plymouth-poweroff.stop /etc/local.d/ # enables plymouth at shutdown screen
```
