<div align="center">
	<img width="256" src="media/logo.svg">
</div>

# dockerpi

[![Docker Pulls](https://badgen.net/docker/pulls/lukechilds/dockerpi?icon=docker&label=Docker%20pulls)](https://hub.docker.com/r/lukechilds/dockerpi/)
[![Docker Image Size](https://badgen.net/docker/size/lukechilds/dockerpi/latest/amd64?icon=docker&label=lukechilds/dockerpi)](https://hub.docker.com/r/lukechilds/dockerpi/tags)
[![GitHub Donate](https://badgen.net/badge/GitHub/Sponsor/D959A7?icon=github)](https://github.com/sponsors/lukechilds)
[![Bitcoin Donate](https://badgen.net/badge/Bitcoin/Donate/F19537?icon=bitcoin)](https://lu.ke/tip/bitcoin)
[![Lightning Donate](https://badgen.net/badge/Lightning/Donate/F6BC41?icon=bitcoin-lightning)](https://lu.ke/tip/lightning)

> A Virtualised Raspberry Pi inside a Docker image

Gives you access to a virtualised ARM based Raspberry Pi machine running the Raspian operating system.

This is not just a Raspian Docker image, it's a full ARM based Raspberry Pi virtual machine environment.

<div style="text-align:center">
	<img alt="demo" src="media/demo.svg" width="720" />
</div>

## Usage

```
docker run -it lukechilds/dockerpi
```

By default all filesystem changes will be lost on shutdown. You can persist filesystem changes between reboots by mounting the `/sdcard` volume on your host:

```
docker run -it -v $HOME/.dockerpi:/sdcard lukechilds/dockerpi
```

If you have a specific image you want to mount you can mount it at `/sdcard/filesystem.img`:

```
docker run -it -v /2019-09-26-raspbian-buster-lite.img:/sdcard/filesystem.img lukechilds/dockerpi
```

If you only want to mount your own image, you can download a much slimmer VM only Docker container that doesn't contain the Raspbian filesystem image:

[![Docker Image Size](https://badgen.net/docker/size/lukechilds/dockerpi/latest/amd64?icon=docker&label=lukechilds/dockerpi:latest)](https://hub.docker.com/r/lukechilds/dockerpi/tags?name=latest)
[![Docker Image Size](https://badgen.net/docker/size/lukechilds/dockerpi/vm/amd64?icon=docker&label=lukechilds/dockerpi:vm)](https://hub.docker.com/r/lukechilds/dockerpi/tags?name=vm)

```
docker run -it -v /2019-09-26-raspbian-buster-lite.img:/sdcard/filesystem.img lukechilds/dockerpi:vm
```

## Which machines are supported?

By default a Raspberry Pi 1 is virtualised, however experimental support has been added for Pi 2 and Pi 3 machines.

You can specify a machine by passing the name as a CLI argument:

```
docker run -it lukechilds/dockerpi pi1
docker run -it lukechilds/dockerpi pi2
docker run -it lukechilds/dockerpi pi3
```

> **Note:** In the Pi 2 and Pi 3 machines, QEMU hangs once the machines are powered down requiring you to `docker kill` the container. See [#4](https://github.com/lukechilds/dockerpi/pull/4) for details.


## Wait, what?

A full ARM environment is created by using Docker to bootstrap a QEMU virtual machine. The Docker QEMU process virtualises a machine with a single core ARM11 CPU and 256MB RAM, just like the Raspberry Pi. The official Raspbian image is mounted and booted along with a modified QEMU compatible kernel.

You'll see the entire boot process logged to your TTY until you're prompted to log in with the username/password pi/raspberry.

```
pi@raspberrypi:~$ uname -a
Linux raspberrypi 5.10.92-v8+ #1514 SMP PREEMPT Mon Jan 17 17:39:38 GMT 2022 aarch64 GNU/Linux
pi@raspberrypi:~$ cat /etc/os-release | head -n 1
PRETTY_NAME="Raspbian GNU/Linux 11 (bullseye)"
pi@raspberrypi:~$ cat /proc/cpuinfo
processor       : 0
BogoMIPS        : 125.00
Features        : fp asimd evtstrm aes pmull sha1 sha2 crc32 cpuid
CPU implementer : 0x41
CPU architecture: 8
CPU variant     : 0x0
CPU part        : 0xd03
CPU revision    : 4

Hardware        : BCM2835
Model           : Raspberry Pi 3 Model B+
pi@raspberrypi:~$ free -h
               total        used        free      shared  buff/cache   available
Mem:           921Mi        42Mi       681Mi       0.0Ki       197Mi       822Mi
Swap:           99Mi          0B        99Mi
```

## Build

Build this image yourself by checking out this repo, `cd` into it and running:

```
docker build -t lukechilds/dockerpi .
```

Build the VM only image with:

```
docker build -t lukechilds/dockerpi:vm --target dockerpi-vm .
```

## Credit

Thanks to [@dhruvvyas90](https://github.com/dhruvvyas90) for his [dhruvvyas90/qemu-rpi-kernel](https://github.com/dhruvvyas90/qemu-rpi-kernel) repo.

## License

MIT © Luke Childs
