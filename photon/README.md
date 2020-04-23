# Photon

Photon is a lightweight, multitasked operating system for OpenComputers.

## Drivers

Photon is separated from other OpenComputers systems in that it provides generic interfaces for all devices, managed through the `drivers` API. Note that the `component` API is mostly not present in userspace; only `component.list` and `component.get` are available, though it has a metatable that will search for drivers when `__index` is called, similar to OpenOS. For certain commonly-used components, such as filesystems, a standard `require`able interface is provided around the `drivers` API.

Sample usage:
```lua
local fs = require("drivers").loadDriver("filesystem") -- in this case, require("filesystem"), or require("component").filesystem work as well

<do stuff with FS>
```

#### Currently included

Photon includes drivers for several devices, with more to come. By default, Photon includes drivers for:

- Redstone cards (driver `redstone`)

- GPUs (driver `gpu`)

- Computronics' Beep, Noise, and Sound cards (driver `sound`)

- Filesystems (driver `filesystem`)

- the Internet card (driver `internet`)

- the Network and Wireless Network cards (driver `modem`)

## The Photon System Architecture

Photon was initially intended to be a microkernel, but certain problems resulted in the decision to leave it monolithic. Some side effects of the initial microkernel design, however, are that no drivers are actually loaded by the kernel; they are all loaded in userspace by various processes.

### The kernel

On boot, the Photon kernel does very little, only initializing the scheduler and basic logging. After the scheduler is loaded, Photon loads the init process from `/sys/core/init.lua`.

### init

Init is where all the interesting stuff happens.

On being started, `init` loads its configuration from `/sys/config/init.cfg`. This configuration specifies scripts to run and services to start.

The default init scripts are:

#### drivers : /sys/core/drivers.lua

Initializes the `drivers` API in `_G`, and disables the `component` API. Note that drivers have full access to `component` as their sole argument.

#### package : /sys/core/package.lua

Initializes the `package` API, plus `loadfile` (reinitialized), `dofile`, and `require`. Removes `unicode`, `computer`, `sched,` and `drivers` from `_G`.

#### io : /sys/core/io.lua

Initializes a non-buffered `io` library. Buffers may happen at some point, but are currently fairly low-priority.

#### os : /sys/core/os.lua

Adds more functions to the `os` API: `os.{get,set}env`, `os.difftime`, `os.exit`, and `os.sleep`.

#### events : /sys/core/events.lua

Initializes a somewhat buggy (largely untested) `event` API.

#### users : /sys/core/users.lua

Initializes the `users` API. This was somewhat of an afterthought, to enable `sudo`ing and file-protection-from-users, but it works.

#### userspace : /sys/core/userspace.lua

Wraps a couple of functions in the `filesystem` driver to enable file protection, starts `rc` services, initializes `print` and `loadfile` (again! this time using the `io` lib), and spawns the shell process. This script also restarts the shell if it dies.

## APIs

Photon includes a decent number of APIs, and should be largely compatible with most OpenOS ones, as long as they don't require the `keyboard` API.

See the [APIs](https://ocawesome101.github.io/photon/APIs.md) section for details.

## RC services

Photon features an `rc` API, allowing for somewhat buggy background service management. Included services:

- `combod.lua`

Adds support for keyboard shortcuts such as `ctrl-C`.

- `cursorblink`

Blinks the cursor. What more do you expect?

- `networkd`

Provides a very buggy (probably broken) FTP protocol.

- `mountd`

Handles automatic mounting and unmounting of filesystem components.
