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
