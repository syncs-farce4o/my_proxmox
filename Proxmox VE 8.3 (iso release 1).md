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

Datacenter->Storage->local-lvm->remove<br/>
local->edit->add all content

## 구독경고 없애기
Datacenter->host->Shell

cp /usr/share/javascript/proxmox-widget-toolkit/proxmoxlib.js /usr/share/javascript/proxmox-widget-toolkit/proxmoxlib.js.bak<br/>
sed -i "s/\tExt.Msg.show/void/g" /usr/share/javascript/proxmox-widget-toolkit/proxmoxlib.js

## windows11 Install on VM

## LXC
pveam update</br>
pveam list local</br>
가능한 리스트중에 하나를 선택</br>
ex) pveam download local ubuntu-24.10-standard_24.10-1_amd64.tar.zst</br>
pveam list local

### docker, nginx
apt install docker.io docker-compose

mkdir -p /data/proxy-manager</br>
cd /data/proxy-manager</br>
vi docker-compose.yml

version: '3'
services:
  app:
    image: 'jc21/nginx-proxy-manager:latest'
    restart: unless-stopped
    ports:
      - '80:80'
      - '81:81'
      - '443:443'
    environment:
      DB_MYSQL_HOST: "db"
      DB_MYSQL_PORT: 3306
      DB_MYSQL_USER: "npm"
      DB_MYSQL_PASSWORD: "npm"
      DB_MYSQL_NAME: "npm"
    volumes:
      - ./data:/data
      - ./config/json:/app/config/production.json
      - ./letsencrypt:/etc/letsencrypt
  db:
    image: 'jc21/mariadb-aria:latest'
    restart: unless-stopped
    environment:
      MYSQL_ROOT_PASSWORD: 'npm'
      MYSQL_DATABASE: 'npm'
      MYSQL_USER: 'npm'
      MYSQL_PASSWORD: 'npm'
    volumes:
      - ./mysql:/var/lib/mysql

docker-compose up -d
