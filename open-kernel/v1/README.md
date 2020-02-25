# Open Kernel 1.0

Despite its name, Open Kernel is a fully fledged operating system for the OpenComputers Minecraft mod.

This documentation covers the boot process and shell commands of Open Kernel 1.0.

## Booting

### Kernel

The kernel stage of Open Kernel's boot process is fairly simple. It defines a component proxy for the root filesystem and a `loadfile` function. The kernel then loads `/boot/libs/term.lua`, which provides a `term` API. After this, the kernel gets a handle to the system log, at `/var/log/kernel.log`. Next, it loads `/boot/libs/filesystem.lua`, which provides a convenience wrapper over `fs.open`.

After this has been done, the kernel defines `_G.kernel`, containing `shutdown` (an alias for `computer.shutdown`)
, `log` (which logs its input to the system logs), and `version`, which return the kernel's version.

Finally, the kernel loads `/sbin/init.lua`, which runs init scripts from `/etc/init.d` and then launches the OpenRC daemon controller.

### Init scripts

## Multitasking

## Shell
