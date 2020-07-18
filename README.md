# Packer template and kickstart File

## Prepare
Install Packer and hypervisor(s). This Packer template has compatible with VirtualBox, VMware Fusion, and vCenter 6.5 or above.
- VirtualBox
  - https://www.virtualbox.org/
- Packer
  - https://www.packer.io/downloads/

## Do
Move to the directory which Packer JSON template file located and runs following command.

```
packer build --only=virtualbox centos8.2.json
```

## VM specs
- 2 vCPUs
- 2048 MB Memory(RAM)
- 30,000 MB Storage with Thin Provisioning
- 1 vNIC with E1000 (on VMware)

## Variables
If you want to change some VM specs, change variables on the config file, or run Packer with `-var` argument(s). For details, please refer to the Packer official document.

This command will build VM with **4vCPUs and 4096MB RAM**  (instead of 2vCPUs and 2048MB RAM.) The Packer JSON template on the "variable" key defines some variables for creating VM. The example of the below overrides `cpus` and `memory_mb` variables. 


```
packer build -var "cpus=4" -var "memory_mb=4096" --only=virtualbox centos8.1.json
```

### For vCenter
You need to set these valiables.
 - `vcenter_server` for vCenter Server's FQDN.
 - `password` for vCenter Server's Password. (with the user which has sufficient privilege. This template sets `administrator@vsphere.local` as the superuser.)
 - `vm_name` for this Virtual Machine name displayed on vCenter Server inventory.
 - `host` or `cluster` for the destination to deploy this Virtual Machine.

## Kickstart Information
 - Network Configuration is configured on kickstart file.
   - IP address is `192.168.1.250/24`
   - Default route is `192.168.1.1`
   - DNS server is `192.168.1.50`
   - Hostname is `localhost.hayaworld.local`
   - If you build on vCenter, please note these IP addresses also defined on Packer variable `boot_vc_ip` and `boot_vc_netmask.`
 - This kickstart file will create `root` and `admin` user with password `VMware1!VMware1!`.
 - Timezone is `Asia/Tokyo`
 - Keyboard layout is `jp`
 - Set SSH configurations using this repositories.
   - https://github.com/cradle8810/sshd_config
   - Please NOTE: This kickstart file configured as put **my SSH public key (hayato@hayabook) on `~admin/.ssh/authorized_hosts`**. Replace this public key to your one, or I can login this vm using my private key(hayato@hayabook).
