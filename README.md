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




