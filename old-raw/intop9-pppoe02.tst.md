# Example: interop9: pppoe server
    
=== "Topology"
    
     <div class="nextWrapper">
         <iframe src="/guides/reference/snippets/next-diagram.html" style="border:none;"></iframe>
     </div>

    
=== "Configuration"
    
    **r1:**
    ```
    hostname r1
    logging file debug ../binTmp/zzz48r1-log.run
    vrf definition tester
     exit
    server telnet tester
     security protocol telnet
     vrf tester
     exit
    vrf def v1
     rd 1:1
     exit
    int di1
     enc ppp
     ppp ip4cp open
     ppp ip6cp open
     ppp ip4cp local 1.1.1.1
     vrf for v1
     ipv4 addr 1.1.1.1 255.255.255.0
     ipv6 addr 1234::1 ffff:ffff:ffff:ffff::
     exit
    int eth1
     p2poe server di1
     exit
    ```
