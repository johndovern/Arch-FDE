# FDE-Arch
These are install scripts for Arch Linux that will give the user a installion
with full disk encryption (FDE) via LVM on LUKS. This is inspired by and ripped
off from Seiichi "Suikan" Horie's script to install ubuntu and void linux with
full disk encryption. His script can be found [here](https://github.com/suikan4github/kaiten-yaki).
## Usage
This is a work in porgress but for the most part a user should be able to do
the following, on bare metal or a vm, while booted from an Arch Linux iso
```bash
# Git is not present on the install iso so it must be installed
pacman -Sy git
git clone https://github.com/johndovern/FDE-Arch.git
cd FDE-Arch
./fde-install
```
I'll smash all of this into a single scritp at some point so that it can be be
curled. Some functionality will be lost, such as the use of config files, but
it could be worth it.

Speaking of functionality what happens after a user runs `./fde-install`?

One area I felt the kaiten yaki script fell short was the **need** to read and
edit the config file to get things working. Users can use and refer to
fde-config for installation but it is not required. Instead the user will be
prompted for any setting that is left blank. The prompts will request the user
to enter the proper information for everything from the target installation
media to their username, password and hostname. Users will not be prompted for
things like their language, time-zone, or other location/user specific info.
This could be added at a later date but this is really just a personal project
so we'll see what happens. After all the prompts have been answered a user can
review their settings. These can be changed until the user is satisfied. After
the user has verified their settings the script will proceed to format their
device, encrypt their device, create a physical volume, and the specified
logical volumes formated as ext4. From here the file systems will be mounted, a
base system will be installed, and the script will chroot into the new system
to configure things like grub and start some services. After all that the user
can reboot into their new installation.
### Optional additions
#### Unlocking with a keyfile at boot
During the prompt proccess the user may be asked if they want to add a keyfile
to an external device (usb stick) to unlock their system at boot. A user can
also reformat this device if they wish. They can choose from vfat, btrfs, ext4,
and xfs. I like vfat best as I can use it accross multiple systems, but you may
want to use this purely with linux and. Either way you've got options.
#### Creating a logical volume for home
Users can pass the `--home` flag to create a seperate logical volume for home.
#### Using a preconfigured fde-config
Like I said before I dislike that my only way to use the kaiten yaki script was
by editing a config file. However, this can be usefull if you want to fork this
repository and maintain your own config. This allows for a faster installation
because the prompts will be bypassed, and you can still reset or change any
settings you like at the verification stage. All this can be done by running
with the `-c` flag. This flag optionally takes a config file. If -c is given
without a file then fde-config will be sourced.
