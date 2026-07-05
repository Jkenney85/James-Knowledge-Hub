
1. Need to find drive ID using the following command

sudo blkid

1. Create mount point

sudo mkdir /data

1. Change ownership, so it can be accessible

sudo groupadd data

sudo usermod -aG data USERNAME (Where USERNAME is the name of the user to be added)

1. Change ownership of the mountpoint

sudo chown -R :data /data

1. Create the automount entry

sudo nano /etc/fstab

UUID=14D82C19D82BF81E /data    auto nosuid,nodev,nofail,x-gvfs-show 0 0

Breaking that line down, we have:

- **UUID=14D82C19D82BF81E** - is the UUID of the drive. You don't have to use the UUID here. You could just use /dev/sdj, but it's always safer to use the UUID as that will never change (whereas the device name could).
- **/data** - is the mount point for the device.
- **auto** - automatically determine the file system
- **nosuid** - specifies that the filesystem cannot contain set userid files. This prevents root escalation and other security issues.
- **nodev** - specifies that the filesystem cannot contain special devices (to prevent access to random device hardware).
- **nofail** - removes the errorcheck.
- **x-gvfs-show** - show the mount option in the file manager. If this is on a GUI-less server, this option won't be necessary.
- **0** - determines which filesystems need to be dumped (0 is the default).
- **0** - determine the order in which filesystem checks are done at boot time (0 is the default).

Save and close the file.

You can test the entry by using the following command before you reboot

sudo mount -a