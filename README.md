# FDE-Arch
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
curl -LJO https://raw.githubusercontent.com/johndovern/FDE-Arch/master/fde-install
chmod +x fde-install
./fde-install
# required if you plan to use the -c flag
curl -LJO https://raw.githubusercontent.com/johndovern/FDE-Arch/master/fde-config
chmod +x fde-config
./fde-install -c
```
By running `./fde-install` the user will then be greeted with a series of
prompts to set things like the target installation disk, their hostname, root
partition size, and several other settings. All settings and brief explanations
of them can be found in
[fde-config.](https://github.com/johndovern/FDE-Arch/blob/master/fde-config)
After the prompt phase users can review their settings. If they messed up or
changed their mind they can choose which settings to reset. Once the user is
satisfied with their settings the installation process will begin. Users will
be prompted once more for a luks password. After that it's off to the races.
Once the script finishes you should be greeted with a message saying the
everything was successful. At this point you should be able to reboot into your
new system without issue.
### Useful but optional
#### Using a preconfigured fde-config
The kaiten-yaki script is nice, but I perfer to do the bare minimum when
installing a system like Arch. If you perfer to edit config files instead of
responding to prompts then please make use of the fed-config file. This is
especially useful if you want to fork this repo and maintain your own
fde-config file. If your file is properly configured the installation process
may be very quick by bypassing the prompts. You can still change any settings
you like at the confirmation stage. All this can be done by running with the
`-c` flag. This flag optionally takes a config file. If -c is given without a
file then fde-config will be sourced. The given file must be propperly
configured (save variable names) or the file will be of little use.
#### Unlocking with a keyfile at boot
During the prompt process the user will be asked if they want to add a keyfile
to an external device to unlock their system at boot. Users can also reformat
this device if they wish. They can choose from vfat, btrfs, ext4, and xfs. I
like vfat best as I can use it across multiple systems, but you may want to use
this purely with linux. Either way you've got options.
#### Changing the installed packages
Please take a look at the `PACKAGES` variable, the third line in fde-install,
to see what packages are installed to the new system. Please edit the file to
add or remove whatever packages you want installed. You may want to edit the
function chroot_install if there are any additional user services you wish to
enable.
#### Creating a logical volume for home
Users can pass the `--home` flag to create a separate logical volume for home.
This is pretty pointless as the user is prompted for, but it's already added.
#### Enabling multilib repo
Users can choose to enable the multilib repo during the prompting phase.
