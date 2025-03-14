# Proxmox VE 8.3 (iso release 1) setting

## Proxmox Install
Install Proxmox VE (Graphical)

I agree

Target Harddisk : /dev/sda(931.51GiB, Samsun SSD 860)<br/>
Filesystem : ext4

Country : Japan<br/>
Time zone : Asia/Tokyo<br/>
Keyboard Layout : U.S. English

Password : ----<br/>
Confirm : ----<br/>
Email : ----

Management Interface : enp4s0<br/>
Hostname (FQDN) : ----<br/>
IP Address (CIDR) : 192.168.0.222<br/>
Gateway : 192.168.0.1<br/>
DNS Server : 192.168.0.1<br/>

check Automatically reboot after successful installation<br/>
Install

## local/local-lvm 통합
Datacenter->host->Shell

lvremove /dev/pve/data<br/>
lvresize -l +100%FREE /dev/pve/root<br/>
resize2fs -p /dev/pve/root

Datacenter->Storage->local-lvm->remove
