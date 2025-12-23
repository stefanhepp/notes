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

- Create new virtual machine
  - Create the container
    ```
    lxc-create --name mycontainer --template download -- --dist debian --release bookworm --arch arm64
    ```
  - Setup the container network configuration in `/var/lib/lxc/<container>`
  - Disable systemd rubbish
    ```
    lxc-start --name mycontainer
    lxc-attach --name mycontainer
    # Run those commands in the container
    # Disable DHCP and systemd resolv automagic
    systemctl stop systemd-resolved
    systemctl disable systemd-resolved
    # Disable network configuration, interferes with static configuration by LXC
    systemctl stop systemd-networkd
    systemctl disable systemd-networkd
    rm /etc/systemd/network/eth0.network
    rm /etc/resolv.conf
    cat <<EOF >/etc/resolv.conf
    search mydomain
    nameserver 8.8.8.8
    EOF
    ```
  - Reboot the container to have LXC static configuration take effect
  - Install the basics
    ```
    apt update
    apt install vim zsh screen openssh-server
    ```

