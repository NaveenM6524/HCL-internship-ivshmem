# Inter-VM Communication using KVM and IVSHMEM

This repository contains a project focused on setting up a virtualized Linux environment using **Kernel-based Virtual Machine (KVM)** and implementing both filesystem-level and memory-level communication between two guest operating systems.

---

## Project Overview

This project focuses on system-level programming in Linux, covering key areas such as virtualization using **KVM**, kernel configuration and compilation, inter-VM file sharing via **Virtio-9p**, and real-time memory communication using **IVSHMEM**. It was implemented on a dual-boot system and involved building a complete environment for efficient file and memory sharing between virtual machines.

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
| `README.md`            | Overview of the project and setup                              |
| `project_log.md`       | Breakdown of tasks and progress notes                          |
| `docs/step-by-step.md` | Detailed technical instructions and command references         |

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

---

## License

This repository is intended for educational and documentation purposes only.
