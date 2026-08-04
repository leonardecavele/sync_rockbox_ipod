This project automatically synchronizes a local music library with a Rockbox iPod when it is connected to a Linux system.

It was designed to make updating the device automatic: plug in the iPod, wait for the synchronization to finish, then unplug it safely.

### How it works

A `udev` rule detects the iPod’s VFAT partition and starts a dedicated `systemd` service.

The service mounts the device at `/mnt/ipod`, verifies that it contains a `.rockbox` directory, then uses `rsync` to mirror the configured music library. `.m3u8` playlists are copied separately.

Files removed from the source library are also removed from the iPod through `rsync --delete`, keeping both libraries consistent.

Once synchronization is complete, pending writes are flushed and the device is unmounted. On supported systems, the power LED indicates activity or an error.

### Installation

The project requires Linux with `systemd`, `udev`, Python 3 and `rsync`.

```bash
sudo python3 install.py
```

The installer copies the scripts, service and `udev` rule to their system locations, then reloads the required services.

### Configuration

Synchronization paths are defined in:

```text
/etc/sync_ipod/config.json
```

`library_root` defines the local library, while the source and destination values select the music and playlist directories relative to the library and iPod mount points.

After editing the configuration, connecting a recognized iPod automatically starts the synchronization.
