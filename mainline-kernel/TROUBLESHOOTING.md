# Mainline Linux kernel - Troubleshooting

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


## Only 2 GiB RAM

If you have the 4 GB model of the Acer Chromebook 13 you'll only see 2
GiB of RAM, you don't have LPAE enabled in your kernel.

The reason for this is that that the Chromebook's RAM does not start
at memory address `0x0` but `0x80000000` (2GiB) Without LPAE the
kernel is only able to adress the first 4GiB and thus will only see
2GiB RAM.

If you have a "black screen"-only boot after enabling LPAE, update to
Linux 4.14 or higher.
