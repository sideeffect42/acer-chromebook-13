# Setting GBB flags


#### Determine flagset

Check [set_gbb_flags.sh](https://chromium.googlesource.com/chromiumos/platform/vboot_reference/+/HEAD/scripts/image_signing/set_gbb_flags.sh) for available options.
Bitwise-OR together the options you want to get your "flagset"

#### Remove write-protect screw

To change the firmware of the Chromebook you need to remove the
write-protect screw.

First of all, turn off the Chromebook.  Then flip over the Chromebook,
catch a screw driver, remove the 13 screws on the bottom and remove
the bottom cover.  When removing the bottom cover, be careful, because
there is a cable going from the mainboard to the headphone connector.

Remove the write-protect screw (`JP10`) between the eDP display
connector and the Wi-Fi antenna cables.

#### Reboot to ChromeOS

While using ChromeOS is not strictly necessary (you could also run
`set_gbb_flags.sh` from whereever you want) if you can I suggest to
boot a ChromeOS.  All the necessary things are right there on
ChromeOS.

When ChromeOS has started, log in as a guest, press `Ctrl + Alt + T`
to open a crosh shell.  In the crosh shell, type `shell` to get a real
shell.


#### Set GBB flags

Note: For this step you will need to know your GBB flag set.

First get root and then execute `set_gbb_flags.sh <your flagset>`

Example:
```sh
sudo su -
/usr/share/vboot/bin/set_gbb_flags.sh 0x39
```

The script will load the firmware from flash, modify and then write it back.
It will also verify the end result.

If all went fine, you can turn the Chromebook back off.

#### Reinsert the write-protect screw

To make sure the firmware is not accidentally written to, you can
screw the write-protect screw (`JP10`) back in.
