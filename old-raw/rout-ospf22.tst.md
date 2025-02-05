# Example: ospf inter area egress filtering with routemap
    
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
    logging file debug ../binTmp/zzz88r1-log.run
    !
    vrf definition tester
     exit
    !
    vrf definition v1
     rd 1:1
     exit
    !
    router ospf4 1
     vrf v1
     router-id 4.4.4.1
     traffeng-id 0.0.0.0
     area 0 enable
     redistribute connected
     exit
    !
    router ospf6 1
     vrf v1
     router-id 6.6.6.1
     traffeng-id ::
     area 0 enable
     redistribute connected
     exit
    !
    interface loopback1
     no description
     vrf forwarding v1
     ipv4 address 2.2.2.1 255.255.255.255
     ipv6 address 4321::1 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff
     no shutdown
     no log-link-change
     exit
    !
    interface loopback2
     no description
     vrf forwarding v1
     ipv4 address 2.2.2.11 255.255.255.255
     ipv6 address 4321::11 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff
     no shutdown
     no log-link-change
     exit
    !
    interface ethernet1
     no description
     vrf forwarding v1
     ipv4 address 1.1.1.1 255.255.255.252
     ipv6 address 1234:1::1 ffff:ffff::
     router ospf4 1 enable
     router ospf4 1 area 0
     router ospf6 1 enable
     router ospf6 1 area 0
     no shutdown
     no log-link-change
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
    logging file debug ../binTmp/zzz88r2-log.run
    !
    route-map p4
     sequence 10 action deny
     sequence 10 match network 2.2.2.8/29 ge 29 le 32
     !
     sequence 20 action permit
     sequence 20 match network 0.0.0.0/0 ge 0 le 32
     !
     exit
    !
    route-map p6
     sequence 10 action deny
     sequence 10 match network 4321::10/124 ge 124 le 128
     !
     sequence 20 action permit
     sequence 20 match network ::/0 ge 0 le 128
     !
     exit
    !
    vrf definition tester
     exit
    !
    vrf definition v1
     rd 1:1
     exit
    !
    router ospf4 1
     vrf v1
     router-id 4.4.4.2
     traffeng-id 0.0.0.0
     area 0 enable
     area 0 route-map-into p4
     area 1 enable
     area 1 route-map-into p4
     redistribute connected
     exit
    !
    router ospf6 1
     vrf v1
     router-id 6.6.6.2
     traffeng-id ::
     area 0 enable
     area 0 route-map-into p6
     area 1 enable
     area 1 route-map-into p6
     redistribute connected
     exit
    !
    interface loopback1
     no description
     vrf forwarding v1
     ipv4 address 2.2.2.2 255.255.255.255
     ipv6 address 4321::2 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff
     no shutdown
     no log-link-change
     exit
    !
    interface loopback2
     no description
     vrf forwarding v1
     ipv4 address 2.2.2.12 255.255.255.255
     ipv6 address 4321::12 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff
     no shutdown
     no log-link-change
     exit
    !
    interface ethernet1
     no description
     vrf forwarding v1
     ipv4 address 1.1.1.2 255.255.255.252
     ipv6 address 1234:1::2 ffff:ffff::
     router ospf4 1 enable
     router ospf4 1 area 0
     router ospf6 1 enable
     router ospf6 1 area 0
     no shutdown
     no log-link-change
     exit
    !
    interface ethernet2
     no description
     vrf forwarding v1
     ipv4 address 1.1.1.5 255.255.255.252
     ipv6 address 1234:2::1 ffff:ffff::
     router ospf4 1 enable
     router ospf4 1 area 1
     router ospf6 1 enable
     router ospf6 1 area 1
     no shutdown
     no log-link-change
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
    
    **r3:**
    ```
    hostname r3
    buggy
    !
    logging file debug ../binTmp/zzz88r3-log.run
    !
    vrf definition tester
     exit
    !
    vrf definition v1
     rd 1:1
     exit
    !
    router ospf4 1
     vrf v1
     router-id 4.4.4.3
     traffeng-id 0.0.0.0
     area 1 enable
     redistribute connected
     exit
    !
    router ospf6 1
     vrf v1
     router-id 6.6.6.3
     traffeng-id ::
     area 1 enable
     redistribute connected
     exit
    !
    interface loopback1
     no description
     vrf forwarding v1
     ipv4 address 2.2.2.3 255.255.255.255
     ipv6 address 4321::3 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff
     no shutdown
     no log-link-change
     exit
    !
    interface loopback2
     no description
     vrf forwarding v1
     ipv4 address 2.2.2.13 255.255.255.255
     ipv6 address 4321::13 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff
     no shutdown
     no log-link-change
     exit
    !
    interface ethernet1
     no description
     vrf forwarding v1
     ipv4 address 1.1.1.6 255.255.255.252
     ipv6 address 1234:2::2 ffff:ffff::
     router ospf4 1 enable
     router ospf4 1 area 1
     router ospf6 1 enable
     router ospf6 1 area 1
     no shutdown
     no log-link-change
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
    
=== "Verification"
    
    ```
    r2#
    r2#
    r2#show ipv4 ospf 1 nei
    r2#show ipv4 ospf 1 nei
     |~~~~~~~~~~~|~~~~~~|~~~~~~~~~|~~~~~~~~~~|~~~~~~~|~~~~~~~~~~|
     | interface | area | address | routerid | state | uptime   |
     |-----------|------|---------|----------|-------|----------|
     | ethernet1 | 0    | 1.1.1.1 | 4.4.4.1  | full  | 00:00:05 |
     | ethernet2 | 1    | 1.1.1.6 | 4.4.4.3  | full  | 00:00:05 |
     |___________|______|_________|__________|_______|__________|
    r2#
    r2#
    ```
    
    ```
    r2#
    r2#
    r2#show ipv6 ospf 1 nei
    r2#show ipv6 ospf 1 nei
     |~~~~~~~~~~~|~~~~~~|~~~~~~~~~~~|~~~~~~~~~~|~~~~~~~|~~~~~~~~~~|
     | interface | area | address   | routerid | state | uptime   |
     |-----------|------|-----------|----------|-------|----------|
     | ethernet1 | 0    | 1234:1::1 | 6.6.6.1  | full  | 00:00:05 |
     | ethernet2 | 1    | 1234:2::2 | 6.6.6.3  | full  | 00:00:05 |
     |___________|______|___________|__________|_______|__________|
    r2#
    r2#
    ```
    
    ```
    r2#
    r2#
    r2#show ipv4 ospf 1 dat 0
    r2#show ipv4 ospf 1 dat 0
     |~~~~~~~~~~|~~~~~~~~~~|~~~~~~~~~~|~~~~~~~~~~~~~~~~|~~~~~|~~~~~~~~~~|
     | routerid | lsaid    | sequence | type           | len | time     |
     |----------|----------|----------|----------------|-----|----------|
     | 4.4.4.1  | 4.4.4.1  | 80000004 | router         | 28  | 00:00:05 |
     | 4.4.4.2  | 4.4.4.2  | 80000007 | router         | 28  | 00:00:02 |
     | 4.4.4.2  | 1.1.1.4  | 80000001 | summaryNetwork | 8   | 00:00:05 |
     | 4.4.4.2  | 2.2.2.3  | 80000001 | summaryNetwork | 8   | 00:00:02 |
     | 4.4.4.1  | 0.0.0.0  | 80000004 | asExternal     | 16  | 01:00:05 |
     | 4.4.4.1  | 1.1.1.0  | 80000001 | asExternal     | 16  | 00:00:05 |
     | 4.4.4.2  | 1.1.1.0  | 80000001 | asExternal     | 16  | 00:00:05 |
     | 4.4.4.2  | 1.1.1.4  | 80000001 | asExternal     | 16  | 00:00:05 |
     | 4.4.4.1  | 2.2.2.1  | 80000001 | asExternal     | 16  | 00:00:05 |
     | 4.4.4.2  | 2.2.2.2  | 80000001 | asExternal     | 16  | 00:00:05 |
     | 4.4.4.1  | 2.2.2.11 | 80000001 | asExternal     | 16  | 00:00:05 |
     | 4.4.4.1  | 4.0.0.0  | 80000001 | opaque-area    | 16  | 00:00:05 |
     | 4.4.4.2  | 4.0.0.0  | 80000001 | opaque-area    | 16  | 00:00:06 |
     |__________|__________|__________|________________|_____|__________|
    r2#
    r2#
    ```
    
    ```
    r2#
    r2#
    r2#show ipv6 ospf 1 dat 0
    r2#show ipv6 ospf 1 dat 0
     |~~~~~~~~~~|~~~~~~~~~~~|~~~~~~~~~~|~~~~~~~~~~~~|~~~~~|~~~~~~~~~~|
     | routerid | lsaid     | sequence | type       | len | time     |
     |----------|-----------|----------|------------|-----|----------|
     | 6.6.6.2  | 681505878 | 80000001 | link       | 24  | 00:00:05 |
     | 6.6.6.2  | 681505879 | 80000002 | link       | 24  | 01:00:05 |
     | 6.6.6.1  | 794298176 | 80000001 | link       | 24  | 00:00:05 |
     | 6.6.6.1  | 0         | 80000003 | router     | 20  | 00:00:05 |
     | 6.6.6.2  | 0         | 80000004 | router     | 20  | 00:00:02 |
     | 6.6.6.2  | 0         | 80000001 | iaPrefix   | 12  | 00:00:05 |
     | 6.6.6.2  | 1         | 80000001 | iaPrefix   | 24  | 00:00:02 |
     | 6.6.6.2  | 681505878 | 80000001 | prefix     | 20  | 00:00:05 |
     | 6.6.6.2  | 681505879 | 80000002 | prefix     | 20  | 01:00:05 |
     | 6.6.6.1  | 794298176 | 80000001 | prefix     | 20  | 00:00:05 |
     | 6.6.6.1  | 0         | 80000003 | asExternal | 16  | 00:00:05 |
     | 6.6.6.2  | 0         | 80000007 | asExternal | 16  | 00:00:05 |
     | 6.6.6.1  | 1         | 80000002 | asExternal | 28  | 00:00:05 |
     | 6.6.6.2  | 1         | 80000005 | asExternal | 16  | 00:00:05 |
     | 6.6.6.1  | 2         | 80000001 | asExternal | 28  | 00:00:05 |
     | 6.6.6.2  | 2         | 80000001 | asExternal | 28  | 00:00:05 |
     | 6.6.6.1  | 0         | 80000001 | rtrInfo    | 16  | 00:00:05 |
     | 6.6.6.2  | 0         | 80000001 | rtrInfo    | 16  | 00:00:06 |
     |__________|___________|__________|____________|_____|__________|
    r2#
    r2#
    ```
    
    ```
    r2#
    r2#
    r2#show ipv4 ospf 1 tre 0
    r2#show ipv4 ospf 1 tre 0
    `--r2
       `--r1
    r2#
    r2#
    ```
    
    ```
    r2#
    r2#
    r2#show ipv6 ospf 1 tre 0
    r2#show ipv6 ospf 1 tre 0
    `--r2
       `--r1
    r2#
    r2#
    ```
    
    ```
    r2#
    r2#
    r2#show ipv4 route v1
    r2#show ipv4 route v1
     |~~~~~~|~~~~~~~~~~~~~|~~~~~~~~|~~~~~~~~~~~|~~~~~~~~~|~~~~~~~~~~|
     | typ  | prefix      | metric | iface     | hop     | time     |
     |------|-------------|--------|-----------|---------|----------|
     | C    | 1.1.1.0/30  | 0/0    | ethernet1 | null    | 00:00:06 |
     | LOC  | 1.1.1.2/32  | 0/1    | ethernet1 | null    | 00:00:06 |
     | C    | 1.1.1.4/30  | 0/0    | ethernet2 | null    | 00:00:06 |
     | LOC  | 1.1.1.5/32  | 0/1    | ethernet2 | null    | 00:00:06 |
     | O E2 | 2.2.2.1/32  | 110/0  | ethernet1 | 1.1.1.1 | 00:00:02 |
     | C    | 2.2.2.2/32  | 0/0    | loopback1 | null    | 00:00:06 |
     | O E2 | 2.2.2.3/32  | 110/0  | ethernet2 | 1.1.1.6 | 00:00:02 |
     | O E2 | 2.2.2.11/32 | 110/0  | ethernet1 | 1.1.1.1 | 00:00:02 |
     | C    | 2.2.2.12/32 | 0/0    | loopback2 | null    | 00:00:06 |
     | O E2 | 2.2.2.13/32 | 110/0  | ethernet2 | 1.1.1.6 | 00:00:02 |
     |______|_____________|________|___________|_________|__________|
    r2#
    r2#
    ```
    
    ```
    r2#
    r2#
    r2#show ipv6 route v1
    r2#show ipv6 route v1
     |~~~~~~|~~~~~~~~~~~~~~~|~~~~~~~~|~~~~~~~~~~~|~~~~~~~~~~~|~~~~~~~~~~|
     | typ  | prefix        | metric | iface     | hop       | time     |
     |------|---------------|--------|-----------|-----------|----------|
     | C    | 1234:1::/32   | 0/0    | ethernet1 | null      | 00:00:06 |
     | LOC  | 1234:1::2/128 | 0/1    | ethernet1 | null      | 00:00:06 |
     | C    | 1234:2::/32   | 0/0    | ethernet2 | null      | 00:00:06 |
     | LOC  | 1234:2::1/128 | 0/1    | ethernet2 | null      | 00:00:06 |
     | O E2 | 4321::1/128   | 110/0  | ethernet1 | 1234:1::1 | 00:00:03 |
     | C    | 4321::2/128   | 0/0    | loopback1 | null      | 00:00:06 |
     | O E2 | 4321::3/128   | 110/0  | ethernet2 | 1234:2::2 | 00:00:03 |
     | O E2 | 4321::11/128  | 110/0  | ethernet1 | 1234:1::1 | 00:00:03 |
     | C    | 4321::12/128  | 0/0    | loopback2 | null      | 00:00:06 |
     | O E2 | 4321::13/128  | 110/0  | ethernet2 | 1234:2::2 | 00:00:03 |
     |______|_______________|________|___________|___________|__________|
    r2#
    r2#
    ```
