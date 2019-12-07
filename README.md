# ESXI-Access-iSCSI-VMFS-Volume
<!-- https://www.synology.com/nl-nl/knowledgebase/DSM/tutorial/Virtualization/How_to_set_up_and_use_iSCSI_target_on_Linux -->

```
sudo apt-get install open-iscsi
sudo nano /etc/iscsi/iscsid.conf
```
Gebruik de opdracht vi om node.startup op automatisch te zetten.

Gebruik de opdracht iscsiadm om iSCSI-detectie te starten.
```
sudo iscsiadm -m discovery -t st -p 172.16.20.1
```
172.16.20.1:3260,-1 iqn.2005-10.org.freenas.ctl:r2d2target
172.16.20.1:3260,-1 iqn.2005-10.org.freenas.ctl:esxi

```
sudo iscsiadm -m node
``````
172.16.20.1:3260,-1 iqn.2005-10.org.freenas.ctl:esxi
172.16.20.1:3260,-1 iqn.2005-10.org.freenas.ctl:r2d2target

Gebruik de opdracht iscsiadm om u aan te melden bij een iSCSI Target.
```
sudo iscsiadm -m node --targetname "iqn.2005-10.org.freenas.ctl:r2d2target" --portal "172.16.20.1:3260" --login
```
Logging in to [iface: default, target: iqn.2005-10.org.freenas.ctl:r2d2target, portal: 172.16.20.1,3260] (multiple)
Login to [iface: default, target: iqn.2005-10.org.freenas.ctl:r2d2target, portal: 172.16.20.1,3260] successful.

List disks
```
sudo fdisk -l
```

# De iSCSI Target ontkoppelen en iSCSI-detectie stoppen
Het volgende gedeelte beschrijft de ontkoppeling van de iSCSI Target en het stoppen van iSCSI-detectie.

Ontkoppel de iSCSI Target en stop iSCSI-detectie met de opdrachten umount en iscsiadm.
cd /
umount /mnt/vm

[root@Synology-FedoraVM /]# iscsiadm -m node --targetname" "iqn.2010-10.synolog y-iscsi:newvirtualdisk.1" --portal" "192.168.0.227:3260" --logout
Logging out of session [sid: 1, target: iqn.2010-10.synology-iscsi:newvirtualdi
sk.1, portal: 192.168.0.227,3260]
Logout of [sid: 1, target: iqn.2010-10.synology-iscsi:newvirtualdisk.1, portal:
192.168.0.227,3260] successful.

[root@Synology-FedoraVM /]# iscsiadm -m discovery --portal "192.168.0.227:3260" --op=delete


# How to Mount/Access VMware VMFS filesystems in Ubuntu Linux
<!-- http://ubuntuguide.net/how-to-mountaccess-vmware-vmfs-filesystems-in-ubuntu-linux -->

VMFS is a clustered filesystem designed to store virtual machine disks for VMware ESX or ESXi Server hosts.

In Ubuntu Linux, there is a command line tool
```
vmfs-tools
```
that allows to access VMFS filesystems from some other non ESX/ESXi host for e.g. maintenance tasks.

For now, only read access is available, but write access is under works. Multiple extents are supported.

## Install Ubuntu

vmfs-tools is available in Ubuntu Software Center.

Or use command:

```
sudo apt-get install vmfs-tools
```

Use vmfs-tools to mount VMFS filesystem

In terminal window, run this to find out your VMware VMFS partition:

```
sudo fdisk -l
```
For example, /dev/sdb1 is the vmfs that we want to mount. Just create a mount point (/mnt/vmfs) and use this command to mount it:

```
mkdir /mnt/vmfs
sudo vmfs-fuse /dev/sda1 /mnt/vmfs
```
Now go on with this filesystem under /mnt/vmfs/
