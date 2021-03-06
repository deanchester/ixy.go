# ixy.go

ixy.go is a Go rewrite of the [ixy](https://github.com/emmericp/ixy) userspace network driver.
It is designed to be readable, idiomatic Go code.
It supports Intel 82599 10GbE NICs (`ixgbe` family).
This is only tested on Debian distributions.

## Features

* just over 1000 lines of Go code for the driver and two sample applications (of which the forwarder is currently not working)
* simple API to use, see this README
* almost matches the speed of the C implementation but with the added features of Go

## Build instructions

You will need the go compiler.
Install using the script provided (ixy.go-install.sh):

```
sudo ./ixy.go-install.sh
```

Go is added to PATH by the script but to use it before a restart execute

```
source ~/.profile
```

The script downloads the repository to ~/go/src/ixy.go:

```
cd ~/go/src/ixy.go
```

Ixy.go uses hugepages. To enable them run:

```
sudo ./setup-hugetlbfs.sh
```

To build the binaries, cd into the folders of the respective application and execute

```
go build
```

The built binaries are located the respective folders.


## Usage

There are two sample applications included in the ixy.go.
You can run the packet generator with

```
sudo ./fwd 0000:AA:BB.C 0000:DD:EE.F
```

The forwarder does not work yet.

The arguments have to the pci slots of the NIC. They can be looked up by using the `lspci` command

### API

All capitalized variables and functions are exported. This includes the following:

device.go:
[IxyInterface](https://github.com/ixy-languages/ixy.go/blob/master/driver/device.go#L13),
[IxyDevice](https://github.com/ixy-languages/ixy.go/blob/master/driver/device.go#L23),
[IxyInit](https://github.com/ixy-languages/ixy.go/blob/master/driver/device.go#L31),
[IxyTxBatchBusy](https://github.com/ixy-languages/ixy.go/blob/master/driver/device.go#L56)

ixgbe.go:
[ReadStats](https://github.com/ixy-languages/ixy.go/blob/master/driver/ixgbe.go#L356),
[RxBatch](https://github.com/ixy-languages/ixy.go/blob/master/driver/ixgbe.go#L380),
[TxBatch](https://github.com/ixy-languages/ixy.go/blob/master/driver/ixgbe.go#L447)

memory.go:
[MemoryAllocateMempool](https://github.com/ixy-languages/ixy.go/blob/master/driver/memory.go#L103),
[PktBufAllocBatch](https://github.com/ixy-languages/ixy.go/blob/master/driver/memory.go#L142),
[PktBufAlloc](https://github.com/ixy-languages/ixy.go/blob/master/driver/memory.go#L156),
[PktBufFree](https://github.com/ixy-languages/ixy.go/blob/master/driver/memory.go#L167)

stats.go:
[DeviceStats](https://github.com/ixy-languages/ixy.go/blob/master/driver/stats.go#L9),
[PrintStats](https://github.com/ixy-languages/ixy.go/blob/master/driver/stats.go#L15),
[PrintStatsDiff](https://github.com/ixy-languages/ixy.go/blob/master/driver/stats.go#L38),
[StatsInit](https://github.com/ixy-languages/ixy.go/blob/master/driver/stats.go#L58)

### Example

`fwd` and `pktgen` contain all sample applications included.

## Internals

`driver/ixgbe.go` contains the core logic.

## License

ixy.go is licensed under the MIT license.

## Disclaimer

ixy.go is not production-ready.
Do not use it in critical environments.
DMA may corrupt memory.

## Other languages

Check out the [other ixy implementations](https://github.com/ixy-languages).
