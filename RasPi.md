# Raspberry Pi Setup and Configuration

Instructions to setup and configure a RaspberryPi computer.

## Loading image file to SD card

Download the latest Raspbian Jesse image from https://www.raspberrypi.org/downloads/ and decompress the zip file to get the img file. (e.g. 2017-04-10-raspbian-jessie-lite.img)

__If using a Mac:__

Before attaching the SD card to your Mac, open `Terminal` and see the list of disks on the system using `diskutil list` and you'll see something like this:
```
> diskutil list

/dev/disk0 (internal):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      GUID_partition_scheme                         251.0 GB   disk0
   1:                        EFI EFI                     314.6 MB   disk0s1
   2:          Apple_CoreStorage Macintosh HD            250.0 GB   disk0s2
   3:                 Apple_Boot Recovery HD             650.0 MB   disk0s3

/dev/disk1 (internal, virtual):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:                  Apple_HFS Macintosh HD           +249.7 GB   disk1
                                 Logical Volume on disk0s2
                                 0453055E-2AF5-4274-852C-465FA8D653B6
                                 Unlocked Encrypted

```

Now, attach the SD card to Mac using a card reader, etc. and run the same command again.

```
> diskutil list

/dev/disk0 (internal):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      GUID_partition_scheme                         251.0 GB   disk0
   1:                        EFI EFI                     314.6 MB   disk0s1
   2:          Apple_CoreStorage Macintosh HD            250.0 GB   disk0s2
   3:                 Apple_Boot Recovery HD             650.0 MB   disk0s3

/dev/disk1 (internal, virtual):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:                  Apple_HFS Macintosh HD           +249.7 GB   disk1
                                 Logical Volume on disk0s2
                                 0453055E-2AF5-4274-852C-465FA8D653B6
                                 Unlocked Encrypted

/dev/disk2 (external, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:     FDisk_partition_scheme                        *16.0 GB    disk2
   1:             Windows_FAT_32 NO NAME                 16.0 GB    disk2s1

```

You will notice that the attached SD card is now visible as `/dev/disk2`. Note this disk number.

Now, unmount the disk using

```
> diskutil unmountDisk /dev/disk2
```

Move to the folder that you have the downloaded image file

```
> cd Downloads
```

Write the image file to the SD card using dd utility

```
sudo dd bs=1m if=<image-file-name> of=/dev/rdisk<disk-number>

```

```
> sudo dd bs=1m if=2017-04-10-raspbian-jessie-lite.img of=/dev/rdisk2

Password:
1237+1 records in
1237+1 records out
1297862656 bytes transferred in 134.500883 secs (9649473 bytes/sec)
```

## Prepare the SD card for SSH connection

To enable SSH, we need to create a file named 'ssh' in the boot folder. (The content of the file is not important.)

```
> sudo touch /Volumes/boot/ssh
```

We can also add our wpa-supplicant.conf file to the boot folder, so that Pi will try connecting to wi-fi at first boot.

```
> sudo touch /Volumes/boot/wpa-supplicant.conf
```

The content of the file should be like:

```
country=TR
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
network={
  ssid="your_ssid"
  psk="your_ssid_passcode"
}
```
You can now unmount the SD card using `diskutil unmountDisk /dev/disk2` or eject from Finder

## Connect the RaspberryPi using ssh

Insert the SD card to Pi and plug-in power. Wait a few seconds to allow Pi to boot. And from Terminal, try pinging the device.

```
> ping raspberrypi.local
```

Once you get a ping, you can try connecting via SSH

```
> ssh pi@raspberrypi.local
```

If you get an error message like

```
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
Someone could be eavesdropping on you right now (man-in-the-middle attack)!
It is also possible that a host key has just been changed.
The fingerprint for the ECDSA key sent by the remote host is
SHA256:Eu38y6Jkr6F1gIvjWh7Pv3sNfwegxKqUR8KxTUYCC/A.
Please contact your system administrator.
Add correct host key in /Users/Mehmet/.ssh/known_hosts to get rid of this message.
Offending ECDSA key in /Users/Mehmet/.ssh/known_hosts:10
ECDSA host key for 192.168.1.7 has changed and you have requested strict checking.
Host key verification failed.
```

... you must remove the previously saved ssh key by running

```
> ssh-keygen -R raspberrypi.local
```
