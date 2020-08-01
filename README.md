# skytraq-datalogger

In 2008 I bought a Skytraq DL-65 bluetooth GPS data logger (55 Euro). The provided software to read tracks from the device is a Windows program the "GPS Photo Tagger" from iTravel-Tech. But since I do not use Windows I needed a software for Linux to download track and configure the device.

When you connect the data logger via USB you can see that it uses a PL2303 USB-serial converter. Without any further software you can dump the NMEA data stream: `cat /dev/ttyUSB0`

My device contains a SkyTraq Venus 6 GPS receiver. Older data loggers contain a Venus 5 GPS receiver. Those should also work with my program. The SkyTraq Venus uses a binary protocol to communicate with the host.

## The software

With the help of these documents I have been able to write a program to talk to the data logger. 

```bash
git clone https://github.com/jze/skytraq-datalogger.git
cd skytraq-datalogger
make
./skytraq-datalogger   # start the program to see the usage information
```

The program works to configure the device and download tracks. There are some things that could be improved:

- change to higher speed on the serial line
- restart transfer of a sector when checksum is incorrect
- timeout when waiting for ACKs

These devices have been reported to work with the program:

- Canmore GT-730F(L)
- SJ-5284S4LG V1.2 (this is my device)

It should also work with other devices using the Skytraq Venus 5 or Skytraq Vensu 6 chipset like:

- SJ-5286A3S
- Navilock BT-455PDL

## Bluetooth

You can also connect the data logger via bluetooth. However, I have not been able to configure the data logger or download tracks via bluetooth.

1. Determine the device's MAC address: hcitool scan
1. rfcomm connect 0 MAC-address
1. Now you can talk to the data logger via /dev/rfcomm0, for example get the NMEA data stream: cat /dev/rfcomm0

output of hcitool info

```
BD Address:  00:0B:0D:00:01:CD
        Device Name: DATALOGG
        LMP Version: 2.0 (0x3) LMP Subversion: 0xbb8
        Manufacturer: Silicon Wave (11)
        Features: 0xff 0xff 0x05 0x38 0x18 0x18 0x00 0x00
                <3-slot packets> <5-slot packets>
```
