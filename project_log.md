# HCL Internship Log

**Internship Title:** Virtualization and Inter-VM Communication using KVM and IVSHMEM 
**Duration:** May 31, 2024 – August 8, 2024 
**Intern:** Naveen M 
**Department:** Electrical and Electronics Engineering, R.M.K. Engineering College

---

## Overview

This internship focused on setting up a virtualized Linux environment using Kernel-based Virtual Machine (KVM) and implementing both filesystem-level and memory-level communication between guest operating systems through Virtio-9p and IVSHMEM (Inter-VM Shared Memory).

The goal was to simulate real-time data exchange and explore virtualization concepts using Ubuntu and Debian virtual machines.

---

## Week-by-Week Breakdown

### Week 1 & 2 (May 31 – June 13, 2024)
**Topic:** Basics of C Programming 
- Learned syntax, control structures, functions, arrays, pointers, and structures in C.
- Practiced memory layout, file I/O operations, and dynamic memory handling.

---

### Week 3 (June 14 – June 20, 2024)
**Topic:** GitHub Setup and Dual Boot Installation 
- Created a GitHub repository for project tracking.
- Practiced basic Git operations (`init`, `add`, `commit`, `push`).
- Installed Ubuntu alongside Windows using a bootable USB (created via Rufus).
- Configured BIOS settings to support dual boot.

---

### Week 4 (June 21 – June 27, 2024)
**Topic:** Kernel Update for Stability 
- Installed required build tools and libraries.
- Downloaded and compiled Linux Kernel 6.12.
- Enabled kernel modules for KVM, 9P filesystem, and IVSHMEM.
- Replaced the default kernel and verified it using `uname -r`.

---

### Week 5 (June 28 – July 4, 2024)
**Topic:** KVM Installation 
- Installed KVM, QEMU, and Virt-Manager on Ubuntu.
- Verified virtualization support using `kvm --version`, `lsmod`, and `/proc/cpuinfo`.

---

### Week 6 (July 5 – July 11, 2024)
**Topic:** Guest OS Setup 
- Installed Ubuntu and Debian as guest operating systems using Virt-Manager.
- Configured CPU, memory, and storage for each VM.
- Enabled VirtIO and host-passthrough for improved performance.

---

### Week 7 (July 12 – July 18, 2024)
**Topic:** Virtio-9p Filesystem Sharing 
- Created a shared folder on the host system.
- Edited the XML configuration of both VMs to mount the shared directory.
- Mounted the shared folder inside guest OSs and verified successful file exchange.

---

### Week 8 (July 19 – July 25, 2024)
**Topic:** IVSHMEM Setup 
- Created a shared memory file in `/dev/shm`.
- Modified VM configurations to include an IVSHMEM device.
- Identified and accessed shared memory region using `lspci` and `devmem2`.

---

### Week 9 (July 26 – August 1, 2024)
**Topic:** Shared Memory Communication using Producer-Consumer Model 
- Developed C programs to implement producer and consumer synchronization.
- Producer (Ubuntu guest) writes data to shared memory.
- Consumer (Debian guest) reads and resets the memory region.
- Validated real-time data synchronization using `/dev/mem` and `mmap()`.

---

### Week 10 (August 2 – August 8, 2024)
**Topic:** Final Review and Documentation 
- Verified kernel stability, VM performance, file and memory communication.
- Documented the full project workflow.
- Prepared final report and committed the complete project to GitHub.

---

## Technologies Used

- **Programming Language:** C
- **Operating System:** Ubuntu (Host), Ubuntu & Debian (Guests)
- **Virtualization Tools:** KVM, QEMU, Virt-Manager
- **Communication Interfaces:** Virtio-9p, IVSHMEM
- **Kernel Tools:** `make`, `menuconfig`, `uname`, `lspci`
- **Debugging Tools:** `devmem2`, `mmap`, `dmesg`

---

## Outcome

This internship provided hands-on experience with Linux system programming, custom kernel builds, and virtualization. Key accomplishments include:

- Successful compilation and deployment of a custom Linux kernel.
- Dual VM setup using KVM with shared file and memory communication.
- Implementation of producer-consumer synchronization across VMs.
- End-to-end understanding of virtualization using KVM and QEMU on Linux.

---

## Author

**Naveen M** 
Intern – HCL Technologies 
Department of Electrical and Electronics Engineering 
R.M.K. Engineering College

