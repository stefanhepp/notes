Virtualization Service Notes
============================

General
-------

Virtualization Software:
- **Docker**: Virtualization of *application* containers. Package and run applications and their dependencies individually (similar to AppImages). 
  
  Docker containers are intended to be automatically built from configuration. Containers are updated by rebuilding with new package versions, testing (via CI/.. testing) and then redeploying. Each application should be its own container.

- **LXC**: Linux virtualization for Linux OS system containers using the host's kernel.

- **Qemu**: Hardware virtualization and emulation. Allows emulation of different architectures and hardware. Emulation of individual programs or whole system.
  Interoperability with KVM or Xen to execute natively without emulation, and emulate hardware devices via Qemu only.

- **KVM**: Kernel module to enable Linux kernel operate as hypervisor. Virtualization of guest systems such as Linux and Windows. Allows hardware paravirtualization via VIRTIO guest drivers.

- **Xen**: Type-1 hardware paravirtualization manager, uses hardware features and guest OS to perform virtualization. Xen virtualization runs in a higher ring, 
  and is managed through the priviledged *dom0* guest. Other guests (*dom1*,..) are virtualized through hardware features and Xen manager.
  Supports Windows virtualization via Intel VT-x or AMD-V. 

- **VirtualBox**, **VMWare**:

- **Hyper-V**:


LXC, LXD
--------


