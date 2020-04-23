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

Photon was initially intended to be a microkernel, but certain problems resulted in the decision to leave it monolithic. Some side effects of the microkernel, however, are that no drivers are actually loaded by the kernel; they are all loaded in userspace by various processes.
