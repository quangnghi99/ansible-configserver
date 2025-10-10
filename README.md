

<h1 align="center">Ansibe to Config Server</h1>

---


## 📝 Table of Contents

- [📝 Table of Contents](#-table-of-contents)
- [🧐 About ](#-about-)
  - [Prerequisites](#prerequisites)
  - [Role Variables](#role-variables)
- [Run Script](#run-script)

## 🧐 About <a name = "about"></a>

This project is used to set up the basic configurations of servers, such as assigning static IPs, mounting disks and NFS, performing system updates, changing passwords, and installing Teleport.

### Prerequisites

Controller node need install Ansible with requirement

- Ansible
- Python3, pip and dependent package

```
pip install -r requirements.txt
```
Note that this role requires root access

### Role Variables
Available variables are listed below:
- Static IP:
Example
```yaml
enable_static_ip: yes
interfaces:
  eth0:
    address: "192.168.0.50/24"
    gateway: "192.168.0.1"
    dns_servers: ["8.8.8.8", "1.1.1.1"]
  eth1:
    address: "10.129.10.100/21"
    routes:
      - to: "10.129.8.0/24"
        via: "10.129.10.1"
```
Set `enable_static_ip: yes` if you want set static for your IP. `eth0` and `eth1` are your interface name. Set address
Set `gateway` will create default route for your interface, if dont set gateway, you can set `routes` when specific routing is needed on the interface.
Set `dns_servers` if you need resolve DNS.
- Mount Disk:
Example
```yaml  
disks:
  - device: "/dev/vdb"
    partition: "/dev/vdb1"
    mountpoint: "/data1"
    fstype: "ext4"
  - device: "/dev/vdc"
    partition: "/dev/vdc1"
    mountpoint: "/data2"
    fstype: "ext4"
```
Each `devide` corresponds to a disk attached to the server. Set `partition`, `mountpoint` and `fstype`. You can add multi device like `vdc`, `vdd`.
- Mount NFS:
Example
```yaml
enable_nfs: yes
nfs_mounts:
  - src: "192.168.0.57:/shares/"
    path: "/mnt/nfs-share"
    fstype: "nfs4"
    opts: "vers=4,minorversion=0,rw,rsize=1048576,wsize=1048576"
```
- Install Teleport
Example
```yaml
enable_teleport: yes
teleport:
  system: "System"
  ip: "192.168.0.50"
  ip_local: "192.168.0.50"
  hostname: "App1"
```
## Run Script
Check interface name:
```bash
ansible-playbook -i inventories/inventory.ini check_network.yml
```
Run script:
```bash
ansible-playbook -i inventories/inventory.ini main.yml
```

