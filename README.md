# Simple TCP Server for Unikraft

This is a simple TCP server running on top of Unikraft.

## Setup

Apart from the current repository, you also need to clone:

* [Unikraft](https://github.com/unikraft/unikraft)
* [lwip](https://github.com/unikraft/lib-lwip)

Make sure each repository is on the `staging` branch.

It is easiest to use a file hierarchy such as:

```
.
|-- apps/
|   `-- app-server/
|-- libs/
|   `-- lwip/
`-- unikraft/
```

If you use a file hierarhy different than the one above, then edit the `Makefile` file and update the variables to point to the correct location:
* Update the `UK_ROOT` variable to point to the location of the Unikraft clone.
* Update the `UK_LIBS` variable to point to the folder storing the library clones.

If you get a `Cannot find library` error at one of the following steps, you may need to add the `lib-` prefix to the libraries listed in the `LIBS` variable.
For instance, `$(UK_LIBS)/lwip` becomes `$(UK_LIBS)/lib-lwip`

## Configure

Configure the build by running:

```
make menuconfig
```

The basic configuration is loaded from the `Config.uk` file.
Save the conifugration and exit.

As a result of this, a `.config` file is created.
A KVM unikernel image will be built from the configuration.

## Build

Build the unikernel KVM image by running:

```
make
```

The resulting image is in the `build/` subfolder, usually named `build/app-server_kvm-x86_64`.

## Run

The resulting image is to be loaded as part of a KVM virtual machine with a software bridge allowing outside network access to the server application.
Start the image by using the `run_server` script (you require `sudo` privileges):

```
$ ./run_server
Creating bridge uk0 if it does not exist ...
Adding IP address 172.44.0.1 to bridge uk0 ...
1: Set IPv4 address 172.44.0.2 mask 255.255.255.0 gw 172.44.0.1
en1: Added
en1: Interface is up
Powered by
o.   .o       _ _               __ _
Oo   Oo  ___ (_) | __ __  __ _ ' _) :_
oO   oO ' _ `| | |/ /  _)' _` | |_|  _)
oOo oOO| | | | |   (| | | (_) |  _) :_
 OoOoO ._, ._:_:_,\_._,  .__,_:_, \___)
                  Tethys 0.5.0~8ef440e0
Listening on port 3333...
[...]
```

The server now accepts packets.
Run the corresponding client from the `client/` folder:

```
$ cd client/

$ make
cc -O2 -Wall -o client client.c

$ ./client 172.44.0.2 3333
Duration: 257 us
Duration: 206 us
Duration: 208 us
Duration: 209 us
Duration: 210 us
Duration: 207 us
[...]
```
