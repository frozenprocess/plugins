# host-device

Move an already-existing device into a container.

## Overview

This simple plugin will move the requested device from the host's network namespace
to the container's. IPAM configuration can be used for this plugin.

## Network configuration reference

The device can be specified with any one of four properties:
* `device`: The device name, e.g. `eth0`, `can0`
* `hwaddr`: A MAC address
* `kernelpath`: The kernel device kobj, e.g. `/sys/devices/pci0000:00/0000:00:1f.6`
* `pciBusID`: A PCI address of network device, e.g `0000:00:1f.6`

For this plugin, `CNI_IFNAME` will be ignored. Upon DEL, the device will be moved back.

The plugin also supports the following [capability argument](https://github.com/containernetworking/cni/blob/master/CONVENTIONS.md):
* `deviceID`: A PCI address of the network device, e.g `0000:00:1f.6`

## Example configuration

A sample configuration with `device` property looks like:

```json
{
	"cniVersion": "0.3.1",
	"type": "host-device",
	"device": "enp0s1"
}
```

A sample configuration with `pciBusID` property looks like:

```json
{
	"cniVersion": "0.3.1",
	"type": "host-device",
	"pciBusID": "0000:3d:00.1"
}
```

A sample configuration utilizing `deviceID` runtime configuration looks like:

1. From operator perspective:
     ```json
    {
    	"cniVersion": "0.3.1",
    	"type": "host-device",
    	"capabilities": {
    		"deviceID":  true
    	}
    }
    ```
2. From plugin perspective:
    ```json
    {
    	"cniVersion": "0.3.1",
    	"type": "host-device",
    	"runtimeConfig": {
    		"deviceID":  "0000:3d:00.1"
    	}
    }
    ```
