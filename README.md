# Add more Storage

- connect USB HDDD 
- ProxLab select disks menu, chek disk name
- open shell

      fdisk /dev/sda
      g
      w

- back to the disks menu
- reload
- Directory 
- Create Directory / 
- Disk: Select dev/sda
- Fyle System: ext4
- Name: Lab_01_Storage_1TB_HDD

# Download ISOs

- select Lab_01_Storage_1TB_HDD
- click ISO Images
- downloads from URL
- bwosrer chek link ubuntu server lite
- paste here URL: https://releases.ubuntu.com/20.04.4/ubuntu-20.04.4-live-server-amd64.iso?_ga=2.167717566.1222274526.1649071264-1485692266.1646825452
- Click: Query URL 
- Click: Downloads

# Create virtual machine

- Create VM ( Blue button, right side on the top )
- Node: base ID: 100m Name: webserver
- Click OS: Do not use any media
- System Bios ( OVMF (UEFI)), EFI storage: local Format: qcow2
- Disk Size: 6 Gb
- Cpu / Type: host, Core:1 Memory: 2048
- Next , next 

# Change CD Ide ( Because of raspberry )

- Rmove Cd / DVD
- Add Cd / DVD : SCSI Storage: Local Image: Ubuntu live server amd64
- Option / Boot Order: 
- SCSI2 pull top, net0 unchack

# Start Vm , and install ubuntu

- Option / Start boot ( yes )
- Start
- Console
- Install ubuntu server
- select etnernet or if proxy
- Mirror default
- Accept full disk , and continue
- Setup Profile
- Install SSH
- Skip other things use tab
- Reboot

# Isntall GEMU

- open other console
- ssh user@ip
- figerprint : yes and give pass

      cat /proc/cpuinfo
      sudo apt update && sudo apt dist-upgrade -y
      sudo apt install qemu-guest-agent
      systemctl status qemu-guest-agent.service
      sudo systemctl start qemu-guest-agent.service
- server option in proxmox , edit enable quemu
  
      sudo poweroof

- Start Vm 

      systemctl status qemu-guest-agent.service
      sudo apt install apache2 

- Open webbroser : my VM ip

To prep the VM 

# Preparing the server for the template

- Connect webserver
  
      ls -l /etc/ssh
      apt search cloud-init
      sudo apt install cloud-init
      cd /etc/ssh 
      sudo rm ssh_host_*
      ls -l
      cat /etc/machine-id  
      sudo truncate -s 0 /etc/machine-id
      sudo ln -s /etc/machine-id /var/lib/dbus/machine-id
      ls -l /var/lib/dbus/machine-id
      cat /etc/machine-id
      sudo apt clean
      sudo apt autoremove
      sudo poweroff

# Create VM Template

- Proxmox VM Right click " Convert to template "
- select hadwere cd/dvd remove iso
- secelet cloud init, edit user name and pass 
- regenerate Image


# Cloning VM

- select Template right click Clone " Full Clone "
- Create Webserver-1 and Webserver-2
- Start Webserver-1
- Connect Webserver-1

      sudo nano /etc/hostname

- rename webserver-1

      sudo nano /etc/hosts

- Change webserver to webserver-1

      sudo reboot

- Repeat this Webserver-2, use diferent name " webserver-2"

# Create Container ( lxc )

- select local storage / CT templates and downloads ubuntu 20.04
- hostname " wwebserver_ct"
- set pass
- Template " ubuntu "
- next, next
- network IPv4 and IPv6 select DHCP
- Next, Finish

# Start Container

- Right click new container, start
- see a container soncole
- user: root
- give pass: " remember setup proc "
- apt install apache2
- prxomox primer concole, connect container ssh root@ ip 
- give ssh " yes "

      adduser midguar
      usermod -aG sudo midguar

- roxmox primer console again connect container ssh midguar@ ip
- open bwoser and use container ip ( checking apache 2)

# Preparing the server for the container template

    sudo apt update && sudo apt dist-upgrade -y
    sudo apt clean
    sudo apt autoremove
    sudo rm ssh_host_*
    ls -l
    sudo truncate -s 0 /etc/machine-id
    sudo ln -s /etc/machine-id /var/lib/dbus/machine-id
    ls -l /var/lib/dbus/machine-id
    cat /etc/machine-id
    sudo poweroff

# Create Container Template

- Proxmox Container Right click " Convert to template "
- select hadwere cd/dvd remove iso
- secelet cloud init, edit user name and pass 
- regenerate Image


# Cloning Container

- select Template right click Clone " Full Clone "
- Create Webserver-ct-1
- Start Webserver-ct-1
- Connect Webserver-ct-1

      cd /etc/ssh
      sudo rm ssh_host_*
      sudo dpkg-reconfigure openssh-server

- Repeat this Webserver-cr-2, use diferent name " webserver-ct-2"