# Example: evpn/cmac over srv6 over bgp additional path
    
=== "Topology"
    
     <div class="nextWrapper">
         <iframe src="/guides/reference/snippets/next-diagram.html" style="border:none;"></iframe>
     </div>

    
=== "Configuration"
    
    **r1:**
    ```
    hostname r1
    buggy
    !
    logging file debug ../binTmp/zzz54r1-log.run
    !
    bridge 1
     rd 1:1
     rt-import 1:1
     rt-export 1:1
     mac-learn
     private-bridge
     exit
    !
    bridge 2
     rd 1:2
     rt-import 1:2
     rt-export 1:2
     mac-learn
     private-bridge
     exit
    !
    vrf definition tester
     exit
    !
    vrf definition v1
     rd 1:1
     exit
    !
    interface bvi1
     no description
     vrf forwarding v1
     ipv4 address 3.3.3.1 255.255.255.252
     ipv6 address 3333::1 ffff::
     no shutdown
     no log-link-change
     exit
    !
    interface bvi2
     no description
     vrf forwarding v1
     ipv4 address 4.4.4.1 255.255.255.252
     ipv6 address 4444::1 ffff::
     no shutdown
     no log-link-change
     exit
    !
    interface ethernet1
     no description
     vrf forwarding v1
     ipv4 address 1.1.1.1 255.255.255.0
     ipv6 address 1234::1 ffff:ffff::
     no shutdown
     no log-link-change
     exit
    !
    interface tunnel1
     no description
     tunnel vrf v1
     tunnel source ethernet1
     tunnel destination 4321:1::
     tunnel mode srv6
     vrf forwarding v1
     ipv6 address 4321:1:: ffff:ffff::
     no shutdown
     no log-link-change
     exit
    !
    router bgp4 1
     vrf v1
     local-as 1
     router-id 4.4.4.1
     address-family evpn
     neighbor 1.1.1.2 remote-as 1
     no neighbor 1.1.1.2 description
     neighbor 1.1.1.2 local-as 1
     neighbor 1.1.1.2 address-family evpn
     neighbor 1.1.1.2 distance 200
     neighbor 1.1.1.2 additional-path-rx evpn
     neighbor 1.1.1.2 additional-path-tx evpn
     neighbor 1.1.1.2 pmsitun
     neighbor 1.1.1.2 segrout
     neighbor 1.1.1.2 send-community standard extended
     afi-evpn 101 bridge-group 1
     afi-evpn 101 srv6 tunnel1
     afi-evpn 101 bmac 001b.2a42.216d
     afi-evpn 101 encapsulation cmac
     afi-evpn 101 update-source ethernet1
     exit
    !
    router bgp6 1
     vrf v1
     local-as 1
     router-id 6.6.6.1
     address-family evpn
     neighbor 1234::2 remote-as 1
     no neighbor 1234::2 description
     neighbor 1234::2 local-as 1
     neighbor 1234::2 address-family evpn
     neighbor 1234::2 distance 200
     neighbor 1234::2 additional-path-rx evpn
     neighbor 1234::2 additional-path-tx evpn
     neighbor 1234::2 pmsitun
     neighbor 1234::2 segrout
     neighbor 1234::2 send-community standard extended
     afi-evpn 102 bridge-group 2
     afi-evpn 102 srv6 tunnel1
     afi-evpn 102 bmac 007a.5219.7206
     afi-evpn 102 encapsulation cmac
     afi-evpn 102 update-source ethernet1
     exit
    !
    !
    !
    !
    !
    !
    !
    !
    !
    !
    !
    !
    !
    !
    !
    !
    ipv6 route v1 4321:2:: ffff:ffff:: 1234::2
    !
    !
    !
    !
    !
    !
    !
    !
    !
    !
    !
    server telnet tester
     security protocol telnet
     no exec authorization
     no login authentication
     vrf tester
     exit
    !
    !
    end
    ```
    
    **r2:**
    ```
    hostname r2
    buggy
    !
    logging file debug ../binTmp/zzz54r2-log.run
    !
    bridge 1
     rd 1:1
     rt-import 1:1
     rt-export 1:1
     mac-learn
     private-bridge
     exit
    !
    bridge 2
     rd 1:2
     rt-import 1:2
     rt-export 1:2
     mac-learn
     private-bridge
     exit
    !
    vrf definition tester
     exit
    !
    vrf definition v1
     rd 1:1
     exit
    !
    interface bvi1
     no description
     vrf forwarding v1
     ipv4 address 3.3.3.2 255.255.255.252
     ipv6 address 3333::2 ffff::
     no shutdown
     no log-link-change
     exit
    !
    interface bvi2
     no description
     vrf forwarding v1
     ipv4 address 4.4.4.2 255.255.255.252
     ipv6 address 4444::2 ffff::
     no shutdown
     no log-link-change
     exit
    !
    interface ethernet1
     no description
     vrf forwarding v1
     ipv4 address 1.1.1.2 255.255.255.0
     ipv6 address 1234::2 ffff:ffff::
     no shutdown
     no log-link-change
     exit
    !
    interface tunnel1
     no description
     tunnel vrf v1
     tunnel source ethernet1
     tunnel destination 4321:2::
     tunnel mode srv6
     vrf forwarding v1
     ipv6 address 4321:2:: ffff:ffff::
     no shutdown
     no log-link-change
     exit
    !
    router bgp4 1
     vrf v1
     local-as 1
     router-id 4.4.4.2
     address-family evpn
     neighbor 1.1.1.1 remote-as 1
     no neighbor 1.1.1.1 description
     neighbor 1.1.1.1 local-as 1
     neighbor 1.1.1.1 address-family evpn
     neighbor 1.1.1.1 distance 200
     neighbor 1.1.1.1 additional-path-rx evpn
     neighbor 1.1.1.1 additional-path-tx evpn
     neighbor 1.1.1.1 pmsitun
     neighbor 1.1.1.1 segrout
     neighbor 1.1.1.1 send-community standard extended
     afi-evpn 101 bridge-group 1
     afi-evpn 101 srv6 tunnel1
     afi-evpn 101 bmac 0076.1b52.2728
     afi-evpn 101 encapsulation cmac
     afi-evpn 101 update-source ethernet1
     exit
    !
    router bgp6 1
     vrf v1
     local-as 1
     router-id 6.6.6.2
     address-family evpn
     neighbor 1234::1 remote-as 1
     no neighbor 1234::1 description
     neighbor 1234::1 local-as 1
     neighbor 1234::1 address-family evpn
     neighbor 1234::1 distance 200
     neighbor 1234::1 additional-path-rx evpn
     neighbor 1234::1 additional-path-tx evpn
     neighbor 1234::1 pmsitun
     neighbor 1234::1 segrout
     neighbor 1234::1 send-community standard extended
     afi-evpn 102 bridge-group 2
     afi-evpn 102 srv6 tunnel1
     afi-evpn 102 bmac 000b.1a3f.1767
     afi-evpn 102 encapsulation cmac
     afi-evpn 102 update-source ethernet1
     exit
    !
    !
    !
    !
    !
    !
    !
    !
    !
    !
    !
    !
    !
    !
    !
    !
    ipv6 route v1 4321:1:: ffff:ffff:: 1234::1
    !
    !
    !
    !
    !
    !
    !
    !
    !
    !
    !
    server telnet tester
     security protocol telnet
     no exec authorization
     no login authentication
     vrf tester
     exit
    !
    !
    end
    ```
