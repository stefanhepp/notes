Networking
==========

Iptables
--------

- Show tracked connection status
  ```
  cat /proc/net/nf_conntrack
  ```
- Note: For masquerading, the server must answer with the same IP address as it received the masqueraded packet from. 
  If the server is reachable by multiple IP addresses, make sure the server answers with the same source IP as the DNAT'ed destination address.

Linux UFW (Simple Firewall configuration)
------------------------------------------

- Add port forwarding from port 1234 to 192.168.1.1 port 56
  - Enable forwarding in `/etc/sysctl.conf` or `/etc/ufw/sysctl.conf`
    ```
    net/ipv4/ip_forward=1
    net/ipv6/conf/default/forwarding=1
    net/ipv6/conf/all/forwarding=1
    ```
  - Add rule to allow incoming port (or via application rule)
    ```
    ufw allow 1234/tcp
    ```
  - Add forwarding rule to destination, using `route` command
    ```
    ufw route allow in on eth0 out on eth1 to 192.168.1.1 proto tcp port 56
    ```
  - Add prerouting and masquerading rules manually, add to `/etc/ufw/before.rules` at the very top (before `*filter`):
    ```
    *nat
    :PREROUTING ACCEPT [0:0]
    -F PREROUTING
    -A PREROUTING -p tcp --dport 1234 -j DNAT --to-destination 192.168.1.1:56
    -F POSTROUTING
    -A POSTROUTING -d 192.168.1.0/24 ! -s 192.168.0.0/16 -j MASQUERADE
    COMMIT
    ```

OpenVPN
-------

- Create CA for OpenVPN server on Linux using easy-rsa:
  - Create CA directory
    ```
    cd /etc/openvpn/
    make-cadir ca
    ```
  - Init root CA
    ```
    cd ca
    ./easy-rsa init-pki
    ./easy-rsa build-ca
    ./easy-rsa gen-dh
    ```
  - Create server certificate
    ```
    ./easy-rsa build-server-full vpn.example.org nopass
    ```
  - Create client certificates
    ```
    ./easy-rsa build-client-full <user>.<device> nopass
    ```

- Bind to IPv6 and IPv4
  - `proto udp` binds to either IPv4 or IPv6 depending on operating system preferences
  - `proto udp6` binds to IPv6 only, but Linux will forward IPv4 to IPv6 sockets if `sysctl net.ipv6.bindv6only` is set to `0`

