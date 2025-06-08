# HCL Internship: Virtualization and Inter-VM Communication using KVM and IVSHMEM

This repository contains the work completed during my 10-week internship at **HCL Technologies**. The project focused on setting up a virtualized Linux environment using **Kernel-based Virtual Machine (KVM)** and implementing both filesystem-level and memory-level communication between two guest operating systems.

---

## Project Overview

The internship provided hands-on experience with system-level programming in Linux. Key areas included virtualization using KVM, kernel configuration and compilation, inter-VM file sharing with **Virtio-9p**, and real-time memory communication using **IVSHMEM**. The project was built on a dual-boot system and involved creating a functional setup for sharing both files and memory between virtual machines.

---

## Technologies and Tools Used

- **Programming Language**: C
- **Operating Systems**: Ubuntu (Host), Ubuntu & Debian (Guests)
- **Virtualization Tools**: QEMU-KVM, Virt-Manager
- **Kernel**: Custom compiled Linux 6.12
- **Communication Mechanisms**: Virtio-9p, IVSHMEM
- **Utilities and Commands**: `devmem2`, `mmap`, `lspci`, `virt-manager`, `virsh`, `make`, `git`

---

## Repository Structure

| File                   | Description                                                    |
|------------------------|----------------------------------------------------------------|
| `README.md`            | Overview of the internship and project setup                  |
| `HCL_Internship_Log.md`| Week-by-week breakdown of tasks completed during the internship |
| `docs/step-by-step.md` | Detailed technical instructions and command references         |

---

## Internship Timeline

| Week       | Focus Area                                                                 |
|------------|-----------------------------------------------------------------------------|
| Week 1–2   | Basics of C programming, including memory layout and file handling         |
| Week 3     | Created GitHub repository and set up Ubuntu dual-boot environment          |
| Week 4     | Downloaded, configured, and compiled the Linux kernel                      |
| Week 5     | Installed KVM and verified virtualization support                          |
| Week 6     | Created and configured guest operating systems using Virt-Manager          |
| Week 7     | Implemented file sharing using Virtio-9p                                   |
| Week 8     | Set up and tested inter-VM shared memory communication with IVSHMEM        |
| Week 9     | Developed and tested producer-consumer synchronization over shared memory  |
| Week 10    | Final testing, documentation, and report preparation                       |

---

## Key Outcomes

- Built and compiled a customized Linux kernel with KVM, Virtio-9p, and IVSHMEM support
- Configured and launched guest virtual machines using KVM
- Enabled cross-VM file sharing using Virtio-9p
- Established real-time inter-VM communication using IVSHMEM
- Developed a memory-based producer-consumer system to demonstrate practical synchronization

---

## Getting Started

To replicate this environment from scratch, refer to the [`docs/step-by-step.md`](./docs/step-by-step.md) file. It covers:

- Kernel download and compilation
- KVM setup and guest OS installation
- Virtio-9p file system integration
- IVSHMEM configuration for memory sharing
- Implementation of C-based producer-consumer synchronization

---

## Author

**Naveen M** 
Department of Electrical and Electronics Engineering 
R.M.K. Engineering College 
Intern at HCL Technologies (May–August 2024)

---

## License

This repository is intended for educational and documentation purposes only.

