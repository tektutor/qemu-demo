## Qemu Demo

## Demo - Installing QEMU in Ubuntu
```
sudo apt update
sudo apt install -y qemu qemu-kvm qemu-system qemu-utils gcc make libvirt-daemon-system libvirt-clients bridge-utils virt-manager
sudo usermod -aG libvirt $(whoami)
newgrp libvirt
egrep -c '(vmx|svm)' /proc/cpuinfo
virsh list --all
qemu-system-x86_64 --version
```


## Demo - Create a VM using QEMU
```
wget https://releases.ubuntu.com/24.04/ubuntu-24.04-desktop-amd64.iso

qemu-img create -f qcow2 ubuntu24.img 20G

qemu-system-x86_64 \
  -boot d \
  -cdrom ubuntu-24.04-desktop-amd64.iso \
  -drive file=ubuntu24.img,format=qcow2 \
  -m 4096 \
  -smp 2 \
  -enable-kvm \
  -vga virtio \
  -net nic -net user
```


## Demo - Serial communication between two VMs over socket
```
# Create disk images for vm1 and vm2
qemu-img create -f qcow2 vm1.img 10G
qemu-img create -f qcow2 vm2.img 10G

# Download ubuntu iso
wget https://releases.ubuntu.com/jammy/ubuntu-22.04.5-live-server-amd64.iso -O ubuntu.iso

# Install ubuntu on vm1
qemu-system-x86_64 -cdrom ubuntu.iso -boot d -hda vm1.img -m 2048

# Install ubuntu on vm2
qemu-system-x86_64 -cdrom ubuntu.iso -boot d -hda vm2.img -m 2048

# Boot vm1 once installation completes
qemu-system-x86_64 -boot c -hda vm1.img -m 2048

# Boot vm2 once installation completes
qemu-system-x86_64 -boot c -hda vm1.img -m 2048

# Enable serial communication in VM1
sudo nano /etc/default/grub
# Modify
GRUB_CMDLINE_LINUX="console=ttyS0"
sudo update-grub
sudo systemctl enable serial-getty@ttyS0.service
sudo reboot

# Enable serial communication in VM2
sudo nano /etc/default/grub
# Modify
GRUB_CMDLINE_LINUX="console=ttyS0"
sudo update-grub
sudo systemctl enable serial-getty@ttyS0.service
sudo reboot

```
