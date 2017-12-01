# Mainline Linux kernel - Troubleshooting

## Only 2 GiB RAM

If you have the 4 GB model of the Acer Chromebook 13 you'll only see 2
GiB of RAM, you don't have LPAE enabled in your kernel.

The reason for this is that that the Chromebook's RAM does not start
at memory address `0x0` but `0x80000000` (2GiB).

Without LPAE, the kernel is only able to adress the first 4GiB and thus will only
see 2GiB RAM, you'll see this in `dmesg`:
```dmesg
OF: fdt: Ignoring memory block 0x100000000 - 0x180000000
```

If you have a "black screen"-only boot after enabling LPAE, update to
Linux 4.14 or higher.


# Black screen at boot and VTs

Sometimes the Tegra DRM driver fails to detect the screen's native
resolution correctly.
If you have a Full-HD Chromebook, it will initialise the framebuffer at a
resolution of 1366x768, which, for some reason, is not supported by the
kernel (a bug?).

To debug, you can add `drm.debug=0xe` to the kernel command line.
On the next boot `dmesg` will show a lot more details about what's going on.

If the framebuffer is initialised using an incorrect resolution, you can add
the following option to the kernel cmdline:
`video=eDP-1:1920x1080-32@60`


## Black screen at boot (not even backlight)

This Chromebook has a PWM driven backlight. So until the PWM driver
has been loaded by the kernel there will not be anything visible on
screen.

```Linux 4.14 Kconfig
Device Drivers --->
	Pulse-Width Modulation (PWM) Support --->
		<*> NVIDIA Tegra PWM support
```

Make sure that `pwm-tegra` is either compiled into the kernel or
loaded early enough for your purposes.


## Chromebook does not turn off on `poweroff`

Make sure you have the AS3722 power-off-driver enabled.

```Linux 4.14 Kconfig
Device Drivers --->
	Board level reset or power off --->
		[*] ams AS3722 power-off-driver
		[*] GPIO restart driver
	Multifunction device drivers --->
		<*> ams AS3722 Power Management IC
```

If enabling the `ams AS3722 power-off driver` didn't help,
make sure that you don't have any conflicting drivers enabled,
like e. g. the `GPIO power-off driver`

Note: While the AS3722 driver is needed to power-off, to reboot you want the
`GPIO restart driver`


## X server crashes randomly when mouse is moved

This is caused by the Mesa swrast driver.
Disable it and the X server should stay alive.

```sh
cd /usr/lib/dri
mkdir ./disabled
mv -v *swrast* ./disabled
```


## Keyboard mapping

```/usr/share/X11/xkb/symbols/inet
partial alphanumeric_keys
xkb_symbols "chromebook" {
//	include "capslock(super)"
//	include "level3(ralt_switch)"
//        key <FK01> {    [ F1, F1, F1, F1, XF86Back ] };
//        key <FK02> {    [ F2, F2, F2, F2, XF86Forward ] };
//        key <FK03> {    [ F3, F3, F3, F3, XF86Reload ] };
//        key <FK04> {    [ F4, F4, F4, F4, F11 ] };
//        key <FK05> {    [ F5, F5, F5, F5, F12 ] };
//        key <FK06> {    [ F6, F6, F6, F6, XF86MonBrightnessDown ] };
//        key <FK07> {    [ F7, F7, F7, F7, XF86MonBrightnessUp ] };
//        key <FK08> {    [ F8, F8, F8, F8, XF86AudioMute ] };
//        key <FK09> {    [ F9, F9, F9, F9, XF86AudioLowerVolume ] };
//        key <FK10> {    [ F10, F10, F10, F10, XF86AudioRaiseVolume ] };
        key <FK01> {    [ F1, F1, XF86Back ] };
        key <FK02> {    [ F2, F2, XF86Forward ] };
        key <FK03> {    [ F3, F3, XF86Reload ] };
        key <FK04> {    [ F4, F4, F11 ] };
        key <FK05> {    [ F5, F5, F12 ] };
        key <FK06> {    [ F6, F6, XF86MonBrightnessDown ] };
        key <FK07> {    [ F7, F7, XF86MonBrightnessUp ] };
        key <FK08> {    [ F8, F8, XF86AudioMute ] };
        key <FK09> {    [ F9, F9, XF86AudioLowerVolume ] };
        key <FK10> {    [ F10, F10, XF86AudioRaiseVolume ] };
        key <BKSP> {    [ BackSpace, BackSpace, Delete, BackSpace ] };

        key <UP>   {    [ Up, Up, Prior, Prior ] };
        key <DOWN> {    [ Down, Down, Next, Next ] };
        key <LEFT> {    [ Left, Left, Home, Home ] };
        key <RGHT> {    [ Right, Right, End, End ] };
        key <LWIN> {    [ Super_L, Super_L, Caps_Lock, Super_L ] };
};
```
