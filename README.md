# apcupsd

A mirror of 3.14.14 of apcupsd but with the patch described here: https://reviews.freebsd.org/D26259

This patch enables apcupsd to correctly read status on certain FreeBSD systems.

I added it for OPNSense specifically as I was getting `BCHARGE` but not `MODEL`, or more importantly `LOADPCT`.  On Linux, these issues were not present, so it has something to do with FreeBSD (or BSD?) specifically.  

## OPNSense

First install the os-apcupsd plugin.

Next, either download the patched binary from releases here or build from source.

### Download

```
# curl -L https://github.com/astr0n8t/apcupsd/releases/download/v1.0/apcupsd-freebsd -o /usr/local/sbin/apcupsd
```

### Compile

```
# git clone https://github.com/astr0n8t/apcupsd.git
# cd apcupsd
# pkg install gmake
# ./configure --enable-modbus-usb --enable-usb
# gmake
# cp src/apcupsd /usr/local/sbin/apcupsd
```

### Fix Config Path

Since the default binary looks in /etc simply link the OPNSense path to the etc path:

```
# ln -s /usr/local/etc/apcupsd/apcupsd.conf /etc/apcupsd/apcupsd.conf
```

### Finish Up

Configure the plugin as needed for your UPS, and you should start seeing those extra fields.

