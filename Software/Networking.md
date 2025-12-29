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
  - Add rule to allow a service from a specific source
    ```
    ufw allow from 192.168.1.1 to any app myservice
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

Server Security
---------------

- Install fail2ban to block repeated ssh failed logins
  ```
  apt install fail2ban
  fail2ban-client status sshd
  ```

DNS, Resolving
--------------

### Have dedicated DNS server for a specific domain
- Install `dnsmasq`
- Edit `/etc/dnsmasq.conf`
  ```
  # disable DHCP
  no-dhcp-interface=
  # read default nameservers from separate conf file
  resolv-file=/etc/resolv-dnsmasq.conf
  # add DNS server for subdomain
  server=/home/192.168.1.1
  server=/1.168.192.in-addr.arpa/192.168.1.1
  ```
- `systemctl restart dnsmasq`
- Edit `/etc/resolv.conf` to use dnsmasq
  ```
  search home
  nameserver 127.0.0.1
  nameserver 8.8.8.8
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
  - Print fingerprint of certificate
    ```
    openssl x509 -fingerprint -sha256 -in pki/issued/<cert>.crt -noout
    ```
  - Renew certificates
    ```
    ./easy-rsa show-expire
    ./easy-rsa renew <user>.<device> nopass
    ```

- Bind to IPv6 and IPv4
  - `proto udp` binds to either IPv4 or IPv6 depending on operating system preferences
  - `proto udp6` binds to IPv6 only, but Linux will forward IPv4 to IPv6 sockets if `sysctl net.ipv6.bindv6only` is set to `0`

- Start configuration at startup
    ```
    # To start /etc/openvpn/client/<name>.conf
    systemctl enable openvpn-client@<name>
    # To start /etc/openvpn/server/<name>.conf
    systemctl enable openvpn-server@<name>
    ```

IPv6
----

### Addresses

Unicast address format:
- 48bit routing prefix, 16bit subnet = 64bit network prefix
- 64bit interface identifier (via DHCP6, random or manual)

Network addresses:
- fe80::/64 : link-local addresses, non-routable; only host interface identifer is used
- fd00::/8  : locally assigned unique addresses (local network, can be routed in same domain); 40bit prefix is randomly assigned, plus 16bit subnet
- ff00::/8  : multicast addresses; 3rd octet is flags, 4th octet is scope
  - ffx1::/16 : interface local
  - ffx2::/16 : link-local (224.0.0.0/24)
  - ffx3::/16 : realm-local (239.255.0.0/16) 
  - ffx8::/16 : organization-local (239.192.0.0/14)
  - ff02::1   : all nodes on local network
  - ff0x::101 : NTP
  - ff0x::181 : PTP messages
  - ff02::6b  : PTP Pdelay messages
- 100::/64  : Discard traffic
- 2000::/3  : Internet address 
- 2001:678::/29 : Provider-independent end user IP addresses
- ::ffff:0:0/96 : IPv4 mapped addresses; shown as e.g. ::ffff:192.168.0.1
- 64:ff9b::/96  : NAT64 IPv6/IPv4 translation (Internet)
- 64:ff9b:1::/48 : local-use NAT64 IPv6/IPv4 translation (local network)
- f02::1:ff00:0/104 : solicited-node link-local multicast address for SLAAC; lower 24bit = from unicast/anycast address
- 2001:db8::/32  : Addresses in documentation, examples
- ::1 : localhost (link-local address)


