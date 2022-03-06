# Arch-FDE
fde-install is bash script for Arch Linux that will give the user an installion
with full disk encryption (FDE) via LVM on LUKS. This is inspired by and ripped
off from Seiichi "Suikan" Horie's kaiten-yaki script to install ubuntu and void
linux with full disk encryption. His script can be found
[here](https://github.com/suikan4github/kaiten-yaki).
## Installation and usage
### fde-install
Booted from a live iso, either in a vm or on bare metal, after establishing an
internet connection run the following:
```bash
curl -O https://raw.githubusercontent.com/johndovern/Arch-FDE/master/fde-install
bash fde-install
# required if you plan to use the -c flag
curl -O https://raw.githubusercontent.com/johndovern/Arch-FDE/master/fde-config
bash fde-install -c
```
By running `bash fde-install` the user will then be greeted with a series of
prompts
1. Set LUKS passphrase
2. Set root password
3. Choose target installation disk
4. Choose to enable the multilib repo
5. Choose to enable TRIM support if the target disk is an SSD
All settings and brief explanations of them can be found in
[fde-config.](https://github.com/johndovern/Arch-FDE/blob/master/fde-config)
After the prompt phase users can review their settings. If they messed up or
changed their mind they can choose which settings to reset. Once the user is
satisfied with their settings the installation process will begin. Once the
script finishes you should be greeted with a message saying the everything was
successful. At this point you should be able to reboot into your new system
without issue.
### Defaults
There are several variables used throughout this script. To streamline things I
have set what I consider to be sane defaults. If installing to a HDD a user
will only need to respond to 4 prompts. If installing to an SSD they will need
to respond to 5 prompts. Bellow are the settings that I have given default
values
```bash
- CRYPTNAME="arch-crypt"
- VGNAME="system"
- LVROOTNAME="root"
- LVROOTSIZE="20G"
- WANTSHOME=1
- LVHOMENAME="home"
- LVHOMESIZE="100" # 100% of the remaining disk will be used
- LVSWAPNAME="swap"
- LVSWAPSIZE="2G"
- EFISIZE="1G"
- TARGETMOUNTPOINT="/mnt/system"
- NEWHOSTNAME="arch"
# Only used if adding a keyfile
- KEYNAME="arch-boot.key"
- KEYMOUNT="/tmp/stick"
- KEYDIR="/keys"
```
### Useful but optional
#### Using a preconfigured fde-config
The kaiten-yaki script is nice, but I prefer to do the bare minimum when
installing a system like Arch. If you prefer to edit config files instead of
responding to prompts then please make use of the fde-config file. This is
especially useful if you want to fork this repo and maintain your own
fde-config file. If your file is properly configured the installation process
may be very quick by bypassing the prompts. You can still change any settings
you like at the confirmation stage. All this can be done by running with the
`-c` flag. This flag optionally takes a config file. If -c is given without a
file then fde-config will be sourced. The given file must be propperly
configured (same variable names) or the file will be of little use. Only use
the -c flag if there is a config file present, otherwise the script will exit.
#### Unlocking with a keyfile at boot
After confirming the main system settings the user will be asked if they want
to add a keyfile to an external device to unlock their system at boot. Users
can also reformat this device if they wish. They can choose from vfat, btrfs,
ext4, and xfs. I like vfat best as I can use it across multiple systems, but
you may want to use this purely with linux. Either way you've got options.
#### Changing the installed packages
Please take a look at the `PACKAGES` variable, the third line in fde-install,
to see what packages are installed to the new system. Please edit the list to
add or remove whatever packages you want installed. You may want to edit the
function chroot_install if there are any additional user services you wish to
enable or disable.
#### No logical volume for home
Users can pass the `--no-home` flag this will set the root volume to use 100%
of the disk left over after swap and possible boot partition. At the confirmation
stage a user can choose to change the size of the root volume, swap volume, or efi
partition.
## Limitations
- The kaiten-yaki script supported overwriting an install. This script does not
  support this and there are no plans to add this functionality.
- Currently root and home will be formated as ext4. I have no plans to change
  this but if you are familiar with btrfs feel free to submit a PR or
  and I will consider implementing btrfs support.
## Disclaimer
The prompts in this script will probably not make sense if you have never
installed Arch, or another bare bones type distro. This script is really meant
for someone who is already familiar with what it is doing but doesn't want to
do everything themselves. I recommend new to Linux users install Arch with full
disk encryption on their own at least once before using this script.
