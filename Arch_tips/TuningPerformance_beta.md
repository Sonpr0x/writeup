# 1. Get mirror server
# 2. Turn off wifi_power save
# 3. Update key & package (sudo pacman -Sy archlinux-keyring chaotic-keyring)
# 4. Tuning boot
# 5. Tuning btrfs (mount / future) ->
mount -o remount,rw,noatime,nodatacow,space_cache=v2,noautodefrag,nodatacow,inode_cache /dev/sdxx
# 6. Turn on trim (ssd) ->
https://superuser.com/questions/1546128/ssd-trim-on-archlinux
https://wiki.archlinux.org/title/Solid_state_drive
# 7. Nvidia driver ->
sudo micro /etc/default/grub
nouveau.modeset=0
sudo update-grub
optimus-manager.startup=MODE
optimus
# 8. Disk cache
# 9. Zram
# 10. Gamemode
https://github.com/FeralInteractive/gamemode
# 11. Watchdog
# 12. Network
https://wiki.archlinux.org/title/Samba#Improve_throughput
#+ Filesystem for SSD
https://en.wikipedia.org/wiki/List_of_file_systems#File_systems_optimized_for_flash_memory.2C_solid_state_media

### Refference
https://wiki.archlinux.org/title/Improving_performance
https://github.com/irqbalance/irqbalance

###
Luu y: Co gang cai dat nhieu cac cong cu tang hieu nang co the lam giam hieu nang hoac gay ra loi.
https://gitlab.com/garuda-linux/themes-and-settings/settings/performance-tweaks
