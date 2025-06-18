## Qemu Demo

## Demo - Installing QEMU in Ubuntu
```
sudo apt update
sudo apt install -y qemu qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils virt-manager
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
