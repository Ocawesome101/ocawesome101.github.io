# Open Kernel 2
Aiming for better documentation than Linux:tm:

Open Kernel 2 is the successor to the original Open Kernel, featuring various new features and improvements. It largely follows the monolithic-kernel design, with most system initialization done in `/boot/kernel.lua`.

## Startup

#### kernel flags
Open Kernel 2 can take a table of kernel flags as its sole argument, such as:
```lua
-- These are the default flags passed by init.lua --
{
  init = "/sbin/init.lua",
  runlevel = 2, -- Max runlevel the system should attempt to reach
  disableLogging = true, -- Enable this option if you're running from a read-only FS or you want faster boot. Disable if you want system logs (i.e. for debugging) and aren't running from a ROFS
  verbose = true, -- Whether to log boot or not, otherwise you will get a black screen until the shell is loaded. Disabling this does seem to slightly improve boot times.
  processTimeout = 0.05 -- The timeout passed to computer.pullSignal in the scheduler. This has a fairly direct impact on performance.
}
```

### early boot
On startup, Open Kernel 2 performs the following actions:
  - stores the kernel startup time
  - creates a proxy for the bootfs
  - creates initial component proxies for the screen and GPU
  - adds helper functions to the GPU api
  - initializes `write()` and `print()` (overwritten later in `/lib/modules/io.lua`)
  - sets up logging
  - sets up filesystem mounts
  - loads /etc/fstab for custom FS mounts
  - sets up several utilities: `loadfile`, `table.new`, `table.copy`, `table.serialize`, `table.iter`, `string.tokenize`, `os.sleep`, and `fs.clean`
  - sets up fancy event handling (the `event` library)
  - sets up a virtual component API for easy connectivity of virtual components (used in the device fs)
  - creates and mounts the device fs
  - initializes the cooperative scheduler and starts `init`, usually from `/sbin/init.lua`

### init
By default, Open Kernel 2 ships with the OpenRC init system.

OpenRC reads its configuration from `/etc/inittab`. This specifies startup scripts and daemons to load.

Note that daemons will only be started if `runlevel` is 2 or higher.

## Scheduler
Open Kernel 2's scheduler is an extension of the `os` API. It adds the following functions for process management.

#### `os.spawn(process: function, name: string): number`
  Spawns a process with name `name` and returns its pid.

#### `os.kill(pid: number)`
  Kills the specified process. Returns `false` and an error message on failure.

#### `os.exit()`
  Kills the current process.

#### `os.pid(): number`
  Returns the current process's PID.
  
#### `os.tasks(): table`
  Returns a table of PIDs of all currently running processes.
  
#### `os.info([pid: number]): table`
  Returns a table of info on the specified process, or the current process if no process is specified.

## Shell

TODO: Document this
