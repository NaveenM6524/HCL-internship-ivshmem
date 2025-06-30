# Step-by-Step Guide: Virtualization and Inter-VM Communication using KVM and IVSHMEM

## OVERVIEW

A virtualized environment was set up on a Linux-based system using Kernel-based Virtual Machine (KVM).

### Tasks Successfully Performed:

* Virtio-9p file sharing between guest operating systems
* Inter-VM Shared Memory (IVSHMEM) communication

This setup enables real-time communication and data sharing between two virtual machines running simultaneously.

---

## STEP 1: Dual Boot Ubuntu OS Installation

**Purpose:** To install Ubuntu alongside Windows operating system to create a dual boot environment.

### Instructions:

1. **Pre-Requisites (Windows):**

   * Turn off BitLocker
   * Turn off Secure Boot in BIOS/UEFI
   * Enable virtualization support in BIOS
   * Allocate free space in Windows for Ubuntu installation
2. Download Ubuntu ISO from: [https://ubuntu.com/download/desktop](https://ubuntu.com/download/desktop)
3. Create a bootable USB drive using Rufus
4. Boot from USB
5. Install Ubuntu alongside Windows

---

## STEP 2: Kernel Modification in Ubuntu

### Commands:

1. Install Required Packages for Kernel Compilation:

```bash
sudo apt update
sudo apt install build-essential libncurses-dev bison flex libssl-dev libelf-dev
```

2. Download the Linux Kernel Source Code:

```bash
mkdir ~/kernel_build
cd ~/kernel_build
wget https://cdn.kernel.org/pub/linux/kernel/v6.x/linux-6.12.tar.xz
tar -xvf linux-6.12.tar.xz
cd linux-6.12
```

3. Copy the current kernel configuration and launch interactive configuration menu:

```bash
cp /boot/config-$(uname -r) .config
make menuconfig
```

### Kernel Configuration Checklist (using `make menuconfig`)

To set up KVM virtualization and enable inter-VM communication using Virtio and IVSHMEM, you'll need to enable several features in the kernel. You can do this using the `make menuconfig` interface.

Enable each option either as **built-in** (`[*]`) or as a **module** (`<M>`), depending on your setup and preferences.

---

### KVM and Virtualization Support

- **KVM (Kernel-based Virtual Machine)** – `[*]`  
  This is the core component needed for running virtual machines on Linux.

- **KVM for Intel or AMD processors** – `[*]`  
  Choose the option that matches your CPU type (Intel or AMD). It’s usually found under the same menu as KVM.

---

### Guest-to-Host Communication (Virtio Drivers)

- **Virtio drivers** – `<M>`  
  Located in: `Device Drivers → Virtio drivers`. These are used for efficient communication between the host and guest VMs.
  

- **Virtio balloon driver** – `<M>`  
  Lets guest machines return unused memory to the host.

---

### Shared Memory and IPC Support

- **System V IPC** – `[*]`  
  Located in: `General setup → System V IPC`. This is necessary for shared memory and other inter-process communication features.

- **POSIX message queues** – `[*]`  
  Located in: `General setup` . Useful for signaling or passing messages between processes using shared memory.

Save the configuration.

### Notes

- Use `[*]` if you want the feature built directly into the kernel.
- Use `<M>` if you prefer it to be a loadable module (safer and more flexible for experimentation).
  
---

4. Modify the configuration to disable trusted key enforcement:

```bash
sudo chmod +x scripts/config
sudo scripts/config --disable SYSTEM_TRUSTED_KEYS
sudo scripts/config --disable SYSTEM_REVOCATION_KEYS
sudo scripts/config --set-str CONFIG_SYSTEM_TRUSTED_KEYS ""
sudo scripts/config --set-str CONFIG_SYSTEM_REVOCATION_KEYS ""
```
  
5. Build the kernel and modules:

```bash
make -j$(nproc)
sudo make modules_install
```

6. Install the New Kernel:

```bash
sudo make install
sudo update-grub
sudo reboot
```

6. Verify New Kernel is Booted:

```bash
uname -r  # Verify new kernel
```

---

## STEP 3: KVM Installation

### Commands:

1. Check if your CPU supports virtualization:
```bash
egrep -c '(vmx|svm)' /proc/cpuinfo
```
if the output is greater than 0, your CPU supports virtualization.
if the output is 0, your CPU does not support virtualization. Enable virtualization in BIOS/UEFI settings.

2. Install KVM and required packages:
```bash
sudo apt update
sudo apt install qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils virt-manager
```

3. Check KVM installation:
```bash
kvm --version
```

4. Check if KVM modules are loaded:
```bash
lsmod | grep kvm
```

5. Add current user to the libvirt and kvm group:
```bash
sudo usermod -aG libvirt $(whoami)
sudo usermod -aG kvm $(whoami)
sudo reboot
```

6.Enable and start the libvirtd service:
```bash
sudo systemctl enable --now libvirtd
```

---

## STEP 4: Install Guest OS (Ubuntu & Debian) using Virt-Manager

### Instructions:

1. Download Ubuntu and Debian ISOs

2. Open Virt-Manager

3. Create new virtual machines for both

4. Select ISO images

5. Choose OS type: Linux

6. Allocate 2 CPUs and 4 GB RAM

7. Create 20 GB virtual disk (QCOW2 recommended)

8. Customize configuration (optional):

   * Add VirtIO devices
   * Set CPU model to host-passthrough

9. Install OS

10. Repeat for second OS

---

## STEP 5: Virtio-9p File System Sharing

**Purpose:** File sharing between guest OSs

### Host (Ubuntu):

```bash
mkdir -p /home/naveen/shared_folder
chmod 777 /home/naveen/shared_folder
```

### Edit VM Config (for both VMs):
Select OS -> Open -> View ->  Overview -> XML 
Add the following xml code at the end before </devices> and apply the changes.

```xml
<filesystem type='mount' accessmode='passthrough'>
  <source dir='/home/naveen/shared_folder'/>
  <target dir='shared'/>
</filesystem>
```
```xml
    <shmem name="ivshmem">
      <model type="ivshmem-plain"/>
      <size unit="M">64</size>
      <address type="pci" domain="0x0000" bus="0x10" slot="0x01" function="0x0"/>
    </shmem>
```

### Inside Each Guest:

```bash
sudo mkdir /mnt/shared_folder
sudo mount -t 9p -o trans=virtio shared /mnt/shared_folder
```

Test by creating a file in one guest and accessing from the other.

---

## STEP 6: Inter-VM Shared Memory Communication Using IVSHMEM

**Purpose:** Enable memory-based communication between two guest OSs using QEMU’s IVSHMEM.

### Host:

```bash
sudo mkdir /dev/shm
sudo touch /dev/shm/ivshmem
sudo chmod 666 /dev/shm/ivshmem
```

### Edit VM Config (both):

```xml
<shmem name='ivshmem'>
  <model type='ivshmem-plain'/>
  <size unit='M'>64</size>
</shmem>
```

### Guest:

```bash
lspci | grep -i memory
lspci -vv -s <device-id>  # Get region 2 memory address
```
IMPORTANT:
Make sure to replace the memory address (e.g., 0x383800000000 or 0xf4000000) in your code with the actual address obtained from this command.

### Install devmem2:

In Ubuntu (guest):

```bash
sudo apt install
sudo apt install devmem2
```

In Debian (guest):

```bash
open nano devmem2.c
```

Run the following code:

```c
#include <stdio.h>
#include <stdlib.h>
#include <stdint.h>
#include <fcntl.h>
#include <sys/mman.h>
#include <unistd.h>
#include <errno.h>
#define MAP_SIZE 4096UL
#define MAP_MASK (MAP_SIZE - 1)
int main(int argc, char **argv) {
 int fd;
 void *map_base, *virt_addr;
 uint64_t target;
 uint32_t read_result;
 off_t target_addr;
 if (argc < 2) {
 printf("Usage: %s <physical address>\n", argv[0]);
 return -1;
 }
 target = strtoull(argv[1], 0, 0);
 target_addr = target;
 if ((fd = open("/dev/mem", O_RDWR | O_SYNC)) == -1) {
 perror("open /dev/mem");
 return -1;
 }
 map_base = mmap(0, MAP_SIZE, PROT_READ | PROT_WRITE, 
MAP_SHARED, fd, target_addr & ~MAP_MASK);
 if (map_base == (void *) -1) {
 perror("mmap");
 close(fd);
 return -1;
 }
 virt_addr = map_base + (target_addr & MAP_MASK);
 read_result = *((uint32_t *) virt_addr);
 printf("Value at address 0x%llx: 0x%08x\n", target, read_result);
 if (munmap(map_base, MAP_SIZE) == -1) {
 perror("munmap");
 }
 close(fd);
 return 0;
}
```

### Compile and Run:

```bash
gcc devmem2.c -o devmem2
sudo ./devmem2 0x383800000000
```

**Memory Address value differs for Ubuntu and Debian, so enter the correct address in each guest OS**

### Write from Ubuntu:

```bash
sudo devmem2 0x383800000000 w 0x12345678
```

### Read from Debian:

```bash
sudo devmem2 0xf40000000
```

---

## STEP 7: Producer-Consumer Synchronization Test

### Ubuntu (Producer):

```c
#include <stdio.h>
#include <stdlib.h>
#include <fcntl.h>
#include <sys/mman.h>
#include <unistd.h>
#include <stdint.h>
#include <time.h>
#define SHM_ADDR 0x383800000000 // ubuntu address
int main() {
 srand(time(NULL));
 int fd = open("/dev/mem", O_RDWR | O_SYNC);
 if (fd < 0) { perror("open"); return -1; }
 volatile uint32_t *shm = (uint32_t *)mmap(NULL, 4096, PROT_READ | 
PROT_WRITE, MAP_SHARED, fd, SHM_ADDR);
 if (shm == MAP_FAILED) { perror("mmap"); return -1; }
 while (1) {
 if (shm[0] == 0) {
 uint32_t data = rand() % 1000;
 shm[1] = data;
 shm[0] = 1;
 printf("Producer: Produced %d\n", data);
 }
 usleep(500000);
 }
 munmap((void *)shm, 4096);
 close(fd);
 return 0;
}
```

### Debian (Consumer):

```c
#include <stdio.h>
#include <stdlib.h>
#include <fcntl.h>
#include <sys/mman.h>
#include <unistd.h>
#include <stdint.h>
#define SHM_ADDR 0xf4000000 // debian address
int main() {
 int fd = open("/dev/mem", O_RDWR | O_SYNC);
 if (fd < 0) { perror("open"); return -1; }
 volatile uint32_t *shm = (uint32_t *)mmap(NULL, 4096, PROT_READ | 
PROT_WRITE, MAP_SHARED, fd, SHM_ADDR);
 if (shm == MAP_FAILED) { perror("mmap"); return -1; }
 while (1) {
 if (shm[0] == 1) {
 uint32_t data = shm[1];
 printf("Consumer: Consumed %d\n", data);
 shm[0] = 0;
 }
 usleep(500000);
 }
 munmap((void *)shm, 4096);
 close(fd);
 return 0;
}
```

Compile and run the consumer first, then the producer.

---

## CONCLUSION

This project demonstrates:

* Setting up a dual boot Linux environment
* Custom kernel compilation
* KVM virtualization setup
* File sharing using Virtio-9p
* Real-time memory-based communication using IVSHMEM
* Producer-consumer memory sync model

