# Example: p4lang: hairpin ingress access list
    
=== "Topology"
    
     <div class="nextWrapper">
         <iframe src="/guides/reference/snippets/next-diagram.html" style="border:none;"></iframe>
     </div>

    
=== "Configuration"
    
    **r1:**
    ```
    hostname r1
    logging file debug ../binTmp/zzz46r1-log.run
    vrf definition tester
     exit
    server telnet tester
     security protocol telnet
     vrf tester
     exit
    vrf def v1
     rd 1:1
     exit
    vrf def v2
     rd 1:2
     exit
    vrf def v9
     rd 1:1
     exit
    hair 1
     ether
     exit
    int lo9
     vrf for v9
     ipv4 addr 10.10.10.227 255.255.255.255
     exit
    int eth1
     vrf for v9
     ipv4 addr 10.11.12.254 255.255.255.0
     exit
    int eth2
     exit
    server dhcp4 eth1
     pool 10.11.12.1 10.11.12.99
     gateway 10.11.12.254
     netmask 255.255.255.0
     dns-server 10.10.10.227
     domain-name p4l
     static 0000.0000.2222 10.11.12.111
     interface eth1
     vrf v9
     exit
    int lo0
     vrf for v1
     ipv4 addr 2.2.2.101 255.255.255.255
     ipv6 addr 4321::101 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff
     exit
    access-list test4
     deny 1 2.2.2.106 255.255.255.255 all 2.2.2.104 255.255.255.255 all
     permit all any all any all
     exit
    access-list test6
     deny 58 4321::106 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff all 4321::104 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff all
     permit all any all any all
     exit
    int lo1
     vrf for v2
     ipv4 addr 2.2.2.100 255.255.255.255
     ipv6 addr 4321::100 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff
     exit
    int sdn1
     vrf for v1
     ipv4 addr 1.1.1.1 255.255.255.0
     ipv6 addr 1234:1::1 ffff:ffff::
     ipv6 ena
     exit
    int sdn2
     vrf for v1
     ipv4 addr 1.1.2.1 255.255.255.0
     ipv6 addr 1234:2::1 ffff:ffff::
     ipv6 ena
     exit
    int sdn3
     vrf for v1
     ipv4 addr 1.1.3.1 255.255.255.0
     ipv6 addr 1234:3::1 ffff:ffff::
     ipv6 ena
     exit
    int sdn4
     vrf for v2
     ipv4 addr 1.1.4.1 255.255.255.0
     ipv6 addr 1234:4::1 ffff:ffff::
     ipv6 ena
     exit
    int hair11
     vrf for v1
     ipv4 addr 1.1.5.1 255.255.255.0
     ipv6 addr 1234:5::1 ffff:ffff::
     ipv6 ena
     ipv4 access-group-in test4
     ipv6 access-group-in test6
     exit
    int hair12
     vrf for v2
     ipv4 addr 1.1.5.2 255.255.255.0
     ipv6 addr 1234:5::2 ffff:ffff::
     ipv6 ena
     exit
    server p4lang p4
     interconnect eth2
     export-vrf v1 1
     export-vrf v2 2
     export-port sdn1 1
     export-port sdn2 2
     export-port sdn3 3
     export-port sdn4 4
     export-port hair11 11
     export-port hair12 12
     vrf v9
     exit
    ipv4 route v1 1.1.4.0 255.255.255.0 1.1.5.2
    ipv6 route v1 1234:4:: ffff:ffff:: 1234:5::2
    ipv4 route v1 2.2.2.100 255.255.255.255 1.1.5.2
    ipv4 route v1 2.2.2.103 255.255.255.255 1.1.1.2
    ipv4 route v1 2.2.2.104 255.255.255.255 1.1.2.2
    ipv4 route v1 2.2.2.105 255.255.255.255 1.1.3.2
    ipv4 route v1 2.2.2.106 255.255.255.255 1.1.5.2
    ipv6 route v1 4321::100 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff 1234:5::2
    ipv6 route v1 4321::103 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff 1234:1::2
    ipv6 route v1 4321::104 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff 1234:2::2
    ipv6 route v1 4321::105 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff 1234:3::2
    ipv6 route v1 4321::106 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff 1234:5::2
    ipv4 route v2 1.1.1.0 255.255.255.0 1.1.5.1
    ipv4 route v2 1.1.2.0 255.255.255.0 1.1.5.1
    ipv4 route v2 1.1.3.0 255.255.255.0 1.1.5.1
    ipv6 route v2 1234:1:: ffff:ffff:: 1234:5::1
    ipv6 route v2 1234:2:: ffff:ffff:: 1234:5::1
    ipv6 route v2 1234:3:: ffff:ffff:: 1234:5::1
    ipv4 route v2 2.2.2.101 255.255.255.255 1.1.5.1
    ipv4 route v2 2.2.2.103 255.255.255.255 1.1.5.1
    ipv4 route v2 2.2.2.104 255.255.255.255 1.1.5.1
    ipv4 route v2 2.2.2.105 255.255.255.255 1.1.5.1
    ipv4 route v2 2.2.2.106 255.255.255.255 1.1.4.2
    ipv6 route v2 4321::101 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff 1234:5::1
    ipv6 route v2 4321::103 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff 1234:5::1
    ipv6 route v2 4321::104 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff 1234:5::1
    ipv6 route v2 4321::105 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff 1234:5::1
    ipv6 route v2 4321::106 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff 1234:4::2
    ```
