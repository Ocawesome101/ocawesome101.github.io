# Open Kernel 1.0

Despite its name, Open Kernel is a fully fledged, single-user operating system for the OpenComputers Minecraft mod.

This documentation covers the boot process and shell commands of Open Kernel 1.0.

## Booting

### Kernel

The kernel stage of Open Kernel's boot process is fairly simple. It defines a component proxy for the root filesystem and a `loadfile` function. The kernel then loads `/boot/libs/term.lua`, which provides a `term` API. After this, the kernel gets a handle to the system log, at `/var/log/kernel.log`. Next, it loads `/boot/libs/filesystem.lua`, which provides a convenience wrapper over `fs.open`.

After this has been done, the kernel defines `_G.kernel`, containing `shutdown` (an alias for `computer.shutdown`)
, `log` (which logs its input to the system logs), and `version`, which return the kernel's version.

Finally, the kernel loads `/sbin/init.lua`, which runs init scripts from `/etc/init.d` and then launches the OpenRC daemon controller.

### Init scripts

By default, the following scripts are placed in `/etc/init.d`:

#### 00_base.lua

Initializes the `colors` API and `_G.sleep`.

#### 10_event.lua

Initializes the `event` API, which is a wrapper around `computer.pullSignal` that supports filtering and event listeners.

#### 20_read.lua

Initializes a `read()` function very similar to `io.read()`, but which supports history and character replacements.

#### 30_errors.lua

Overwrites the default `error` function to only print the error, rather than crash. Also initializes `printError`. Has not been tested particularly well.

#### 40_tables.lua

Adds `table.new` and `table.copy` to the `table` API. `table.new` returns a table of its arguments, with the metatable set to `{ __index = table }`. This allows for phrases such as `local tbl = table.new("1", "2", "3"); tbl:insert("4")`, rather than `local tbl = {"1", "2", "3"}; table.insert(tbl, "4")`. `table.copy` returns a copy of the provided table.

#### 50_modules.lua

Initializes the `module` API, `_G.require`, and `_G.dofile`. These are very similar to the default Lua `package` API.

#### 60_networking.lua

Initializes the `network` API to manage hostnames. Also creates `internet` if it finds an Internet Card.

#### 70_filesystems.lua

Initializes filesystem mounting under `/mnt`. Seems to be slightly buggy.

#### 80_multitasking.lua

Initializes the (rather buggy) cooperative task scheduler.

### OpenRC

OpenRC provides the `rc` API, which manages background services, such as the default `shellrestartd`, which restarts the shell if it crashes.

## Multitasking

Open Kernel's cooperative scheduler is in more or less an observation stage. It doesn't really work, likely due to a bug in the shell, but it doesn't crash and burn either.

## Shell

TODO: Add shell docs
