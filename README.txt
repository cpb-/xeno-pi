

# How to compile a Linux kernel with Xenomai support (I-pipe-core patch) for Raspberry Pi.

## List the available pre and post patches

```
$ git tag
4.9.51
4.19.82
```

## Select the pre and post patches you want

```
$ git checkout 4.19.82
```


## Download the Raspberry Pi-specific Linux kernel sources

```
$ git clone https://github.com/raspberrypi/linux  rpi-linux
```

## Download the I-pipe-core patch

```
$ wget https://xenomai.org/downloads/ipipe/v4.x/arm/ipipe-core-4.19.82-arm-6.patch
```

## Download and extract the Xenomai tarball

```
$ wget https://xenomai.org/downloads/xenomai/stable/xenomai-3.1.tar.bz2
$ tar xf xenomai-3.1.tar.bz2
$ cd xenomai-3.1/
$ patch -p1 < ../add-arm-8-a-architecture-to-xenomai-3.1.patch
```

## Select a Linux kernel commit with respect to the I-pipe version

```
$ cd linux-rpi
$ git checkout eb29974a
```

## Apply all the patches in a dedicated branch

```
$ git checkout -b RPI-I-pipe-core-4.19.82
$ patch -p1 < ../pre-ipipe-core-4.19.82-arm-6.patch
$ patch -p1 < ../ipipe-core-4.19.82-arm-6.patch
$ patch -p1 < ../post-ipipe-core-4.19.82-arm-6.patch
$ git add -A
$ git commit -a -m "RPI-I-pipe-core-4.19.82 patches applied."
```

Please use first the `--dry-run` option of the `patch` command to ensure correct patch application (with some warnings, but no errors).

## Apply the Xenomai modifications on the Linux sources

```
$ ../xenomai-3.1/scripts/prepare-kernel --arch=arm --linux=.
```

## Prepare the Linux kernel configuration and compile it

```
$ make ARCH=arm bcm2709_defconfig   # For Raspberry Pi 2 and 3
```

or

```
$ make ARCH=arm bcm2711_defconfig   # For Raspberry Pi 4
```

Then

```
$ make ARCH=arm menuconfig
$ make ARCH=arm CROSS_COMPILE=arm-linux-
```

