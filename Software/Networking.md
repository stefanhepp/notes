Networking
==========

Linux UFW (Simpole Firewall configuration)
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
