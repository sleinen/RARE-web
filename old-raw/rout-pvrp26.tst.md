# Example: pvrp aggregation
    
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
    logging file debug ../binTmp/zzz71r1-log.run
    !
    route-map p4
     sequence 10 action deny
     sequence 10 match network 2.2.2.12/32 ge 32 le 32
     !
     sequence 20 action permit
     sequence 20 match network 0.0.0.0/0 ge 0 le 32
     !
     exit
    !
    route-map p6
     sequence 10 action deny
     sequence 10 match network 4321::12/128 ge 128 le 128
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
    router pvrp4 1
     vrf v1
     router-id 4.4.4.1
     redistribute connected
     exit
    !
    router pvrp6 1
     vrf v1
     router-id 6.6.6.1
     redistribute connected
     exit
    !
    interface loopback0
     no description
     vrf forwarding v1
     ipv4 address 2.2.2.1 255.255.255.255
     ipv6 address 4321::1 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff
     no shutdown
     no log-link-change
     exit
    !
    interface loopback1
     no description
     vrf forwarding v1
     ipv4 address 2.2.2.11 255.255.255.255
     ipv6 address 4321::11 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff
     no shutdown
     no log-link-change
     exit
    !
    interface loopback2
     no description
     vrf forwarding v1
     ipv4 address 2.2.2.21 255.255.255.255
     ipv6 address 4321::21 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff
     no shutdown
     no log-link-change
     exit
    !
    interface ethernet1
     no description
     vrf forwarding v1
     ipv4 address 1.1.1.1 255.255.255.252
     ipv6 address 1234:1::1 ffff:ffff::
     router pvrp4 1 enable
     router pvrp4 1 route-map-in p4
     router pvrp6 1 enable
     router pvrp6 1 route-map-in p6
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
    logging file debug ../binTmp/zzz71r2-log.run
    !
    vrf definition tester
     exit
    !
    vrf definition v1
     rd 1:1
     exit
    !
    router pvrp4 1
     vrf v1
     router-id 4.4.4.2
     redistribute connected
     aggregate 2.2.2.0/24
     exit
    !
    router pvrp6 1
     vrf v1
     router-id 6.6.6.2
     redistribute connected
     aggregate 4321::/32
     exit
    !
    interface loopback0
     no description
     vrf forwarding v1
     ipv4 address 2.2.2.2 255.255.255.255
     ipv6 address 4321::2 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff
     no shutdown
     no log-link-change
     exit
    !
    interface loopback1
     no description
     vrf forwarding v1
     ipv4 address 2.2.2.12 255.255.255.255
     ipv6 address 4321::12 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff
     no shutdown
     no log-link-change
     exit
    !
    interface loopback2
     no description
     vrf forwarding v1
     ipv4 address 2.2.2.22 255.255.255.255
     ipv6 address 4321::22 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff
     no shutdown
     no log-link-change
     exit
    !
    interface ethernet1
     no description
     vrf forwarding v1
     ipv4 address 1.1.1.2 255.255.255.252
     ipv6 address 1234:1::2 ffff:ffff::
     router pvrp4 1 enable
     router pvrp6 1 enable
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
    r2#show ipv4 pvrp 1 sum
    r2#show ipv4 pvrp 1 sum
     |~~~~~~~~~~~|~~~~~~~~~|~~~~~~|~~~~~~~~~~~|~~~~~~~~~|~~~~~~~~~|~~~~~~~~~~|~~~~~~~~~~|
     | iface     | router  | name | peerif    | peer    | learned | adverted | uptime   |
     |-----------|---------|------|-----------|---------|---------|----------|----------|
     | ethernet1 | 4.4.4.1 | r1   | ethernet1 | 1.1.1.1 | 3       | 4        | 00:00:08 |
     |___________|_________|______|___________|_________|_________|__________|__________|
    r2#
    r2#
    ```
    
    ```
    r2#
    r2#
    r2#show ipv6 pvrp 1 sum
    r2#show ipv6 pvrp 1 sum
     |~~~~~~~~~~~|~~~~~~~~~|~~~~~~|~~~~~~~~~~~|~~~~~~~~~~~|~~~~~~~~~|~~~~~~~~~~|~~~~~~~~~~|
     | iface     | router  | name | peerif    | peer      | learned | adverted | uptime   |
     |-----------|---------|------|-----------|-----------|---------|----------|----------|
     | ethernet1 | 6.6.6.1 | r1   | ethernet1 | 1234:1::1 | 3       | 4        | 00:00:08 |
     |___________|_________|______|___________|___________|_________|__________|__________|
    r2#
    r2#
    ```
    
    ```
    r2#
    r2#
    r2#show ipv4 pvrp 1 rou
    r2#show ipv4 pvrp 1 rou
     |~~~~~~|~~~~~~~~~~~~~|~~~~~~~~|~~~~~~~~~~~|~~~~~~~~~|~~~~~~~~~~|
     | typ  | prefix      | metric | iface     | hop     | time     |
     |------|-------------|--------|-----------|---------|----------|
     | C    | 1.1.1.0/30  | 1/0    | ethernet1 | null    | 00:00:05 |
     | P    | 2.2.2.0/24  | 0/0    | null      | null    | 00:00:05 |
     | null | 2.2.2.1/32  | 80/10  | ethernet1 | 1.1.1.1 | 00:00:05 |
     | C    | 2.2.2.2/32  | 2/0    | loopback0 | null    | 00:00:14 |
     | null | 2.2.2.11/32 | 80/10  | ethernet1 | 1.1.1.1 | 00:00:05 |
     | C    | 2.2.2.12/32 | 2/0    | loopback1 | null    | 00:00:14 |
     | null | 2.2.2.21/32 | 80/10  | ethernet1 | 1.1.1.1 | 00:00:05 |
     | C    | 2.2.2.22/32 | 2/0    | loopback2 | null    | 00:00:14 |
     |______|_____________|________|___________|_________|__________|
    r2#
    r2#
    ```
    
    ```
    r2#
    r2#
    r2#show ipv6 pvrp 1 rou
    r2#show ipv6 pvrp 1 rou
     |~~~~~~|~~~~~~~~~~~~~~|~~~~~~~~|~~~~~~~~~~~|~~~~~~~~~~~|~~~~~~~~~~|
     | typ  | prefix       | metric | iface     | hop       | time     |
     |------|--------------|--------|-----------|-----------|----------|
     | C    | 1234:1::/32  | 1/0    | ethernet1 | null      | 00:00:02 |
     | P    | 4321::/32    | 0/0    | null      | null      | 00:00:02 |
     | null | 4321::1/128  | 80/10  | ethernet1 | 1234:1::1 | 00:00:02 |
     | C    | 4321::2/128  | 2/0    | loopback0 | null      | 00:00:14 |
     | null | 4321::11/128 | 80/10  | ethernet1 | 1234:1::1 | 00:00:02 |
     | C    | 4321::12/128 | 2/0    | loopback1 | null      | 00:00:14 |
     | null | 4321::21/128 | 80/10  | ethernet1 | 1234:1::1 | 00:00:02 |
     | C    | 4321::22/128 | 2/0    | loopback2 | null      | 00:00:14 |
     |______|______________|________|___________|___________|__________|
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
     | C    | 1.1.1.0/30  | 0/0    | ethernet1 | null    | 00:00:14 |
     | LOC  | 1.1.1.2/32  | 0/1    | ethernet1 | null    | 00:00:14 |
     | P    | 2.2.2.0/24  | 0/0    | null      | null    | 00:00:06 |
     | P EX | 2.2.2.1/32  | 80/10  | ethernet1 | 1.1.1.1 | 00:00:06 |
     | C    | 2.2.2.2/32  | 0/0    | loopback0 | null    | 00:00:14 |
     | P EX | 2.2.2.11/32 | 80/10  | ethernet1 | 1.1.1.1 | 00:00:06 |
     | C    | 2.2.2.12/32 | 0/0    | loopback1 | null    | 00:00:14 |
     | P EX | 2.2.2.21/32 | 80/10  | ethernet1 | 1.1.1.1 | 00:00:06 |
     | C    | 2.2.2.22/32 | 0/0    | loopback2 | null    | 00:00:14 |
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
     | C    | 1234:1::/32   | 0/0    | ethernet1 | null      | 00:00:14 |
     | LOC  | 1234:1::2/128 | 0/1    | ethernet1 | null      | 00:00:14 |
     | P    | 4321::/32     | 0/0    | null      | null      | 00:00:02 |
     | P EX | 4321::1/128   | 80/10  | ethernet1 | 1234:1::1 | 00:00:02 |
     | C    | 4321::2/128   | 0/0    | loopback0 | null      | 00:00:14 |
     | P EX | 4321::11/128  | 80/10  | ethernet1 | 1234:1::1 | 00:00:02 |
     | C    | 4321::12/128  | 0/0    | loopback1 | null      | 00:00:14 |
     | P EX | 4321::21/128  | 80/10  | ethernet1 | 1234:1::1 | 00:00:02 |
     | C    | 4321::22/128  | 0/0    | loopback2 | null      | 00:00:14 |
     |______|_______________|________|___________|___________|__________|
    r2#
    r2#
    ```
