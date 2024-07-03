### ZFS Cheat Sheet for Ubuntu on RPi5

#### 1. **Installing ZFS on Ubuntu**

```bash
sudo apt update
sudo apt install zfsutils-linux
```

#### 2. **Creating a ZFS Pool with RAIDZ2 and `ashift=16`**

```bash
sudo zpool create -o ashift=16 sdr raidz2 /dev/sda /dev/sdb /dev/sdc /dev/sdd /dev/sde /dev/sdf /dev/sdg /dev/sdh
```

#### 3. **Creating a ZFS Dataset**

```bash
sudo zfs create sdr/nas
```

#### 4. **Setting Compression and Optimizations**

- **Set `lz4` Compression:**

```bash
sudo zfs set compression=lz4 sdr/nas
```

- **Set `recordsize` to 64K:**

```bash
sudo zfs set recordsize=64k sdr/nas
```

#### 5. **Mounting ZFS Datasets**

- **ZFS datasets are automatically mounted. To verify:**

```bash
sudo zfs get mountpoint sdr/nas
```

- **If you need to set or change the mount point:**

```bash
sudo zfs set mountpoint=/mnt/nas sdr/nas
```

#### 6. **Ensuring ZFS Automount After Reboot**

- **Enable ZFS services on systemd-based systems:**

```bash
sudo systemctl enable zfs.target
sudo systemctl enable zfs-import-cache
sudo systemctl enable zfs-mount
sudo systemctl enable zfs-import.target
```

- **Update the ZFS cache:**

```bash
sudo zpool set cachefile=/etc/zfs/zpool.cache sdr
```

#### 7. **Verifying the Pool and Dataset**

- **Check pool status:**

```bash
sudo zpool status
```

- **Check dataset usage:**

```bash
sudo zfs list
```

#### 8. **Replacing Faulty Drives**

- **Identify faulty drive:**

```bash
sudo zpool status
```

- **Offline the faulty drive:**

```bash
sudo zpool offline sdr /dev/sdX
```

- **Replace the drive:**

```bash
sudo zpool replace sdr /dev/sdX /dev/sdY
```

- **Online the new drive:**

```bash
sudo zpool online sdr /dev/sdY
```

#### 9. **Setting SSD Optimization Options**

- **Disable Atime:**

```bash
sudo zfs set atime=off sdr/nas
```

- **Enable autotrim for the pool:**

```bash
sudo zpool set autotrim=on sdr
```

#### 10. **Manual Sync of Writes**

- **Force synchronous writes for a dataset (if needed):**

```bash
sudo zfs set sync=always sdr/nas
```

- **Manually sync pending writes:**

```bash
sudo zfs sync sdr
```

- **Flush filesystem buffers:**

```bash
sync
```

### Additional Tips

- **Snapshot a dataset:**

```bash
sudo zfs snapshot sdr/nas@snapshot1
```

- **List snapshots:**

```bash
sudo zfs list -t snapshot
```

- **Rollback to a snapshot:**

```bash
sudo zfs rollback sdr/nas@snapshot1
```

- **Rename a dataset:**

```bash
sudo zfs rename sdr/nas sdr/newname
```

### Summary
1. **Install ZFS**: `sudo apt install zfsutils-linux`
2. **Create RAIDZ2 Pool with `ashift=16`**: `sudo zpool create -o ashift=16 sdr raidz2 /dev/sda /dev/sdb /dev/sdc /dev/sdd /dev/sde /dev/sdf /dev/sdg /dev/sdh`
3. **Create Dataset**: `sudo zfs create sdr/nas`
4. **Set Compression & Recordsize**: `sudo zfs set compression=lz4 sdr/nas`, `sudo zfs set recordsize=64k sdr/nas`
5. **Disable Atime**: `sudo zfs set atime=off sdr/nas`
6. **Change Mount Point**: `sudo zfs set mountpoint=/mnt/nas sdr/nas`
7. **Enable ZFS Services for Automount**: `sudo systemctl enable zfs.target`, `sudo systemctl enable zfs-import-cache`, `sudo systemctl enable zfs-mount`, `sudo systemctl enable zfs-import.target`
8. **Update ZFS Cache**: `sudo zpool set cachefile=/etc/zfs/zpool.cache sdr`
9. **Monitor Pool and Dataset**: `sudo zpool status`, `sudo zfs list`
10. **Replace Faulty Drive**: `sudo zpool offline sdr /dev/sdX`, `sudo zpool replace sdr /dev/sdX /dev/sdY`, `sudo zpool online sdr /dev/sdY`
11. **Enable Trim**: `sudo zpool set autotrim=on sdr`, `sudo zfs set autotrim=on sdr/nas`
12. **Manual Sync**: `sudo zfs sync sdr`, `sync`
