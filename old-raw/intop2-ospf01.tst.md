# Example: interop2: point2point ospf
    
=== "Topology"
    
     <div class="nextWrapper">
         <iframe src="/guides/reference/snippets/next-diagram.html" style="border:none;"></iframe>
     </div>

    
=== "Configuration"
    
    **r1:**
    ```
    hostname r1
    logging file debug ../binTmp/zzz59r1-log.run
    vrf definition tester
     exit
    server telnet tester
     security protocol telnet
     vrf tester
     exit
    vrf def v1
     rd 1:1
     exit
    router ospf4 1
     vrf v1
     router 4.4.4.1
     area 0 ena
     red conn
     exit
    router ospf6 1
     vrf v1
     router 6.6.6.1
     area 0 ena
     red conn
     exit
    int eth1
     vrf for v1
     ipv4 addr 1.1.1.1 255.255.255.0
     ipv6 addr fe80::1 ffff::
     router ospf4 1 ena
     router ospf6 1 ena
     exit
    int lo0
     vrf for v1
     ipv4 addr 2.2.2.1 255.255.255.255
     ipv6 addr 4321::1 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff
     exit
    ```
