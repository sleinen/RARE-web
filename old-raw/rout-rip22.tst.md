# Example: rip prefix withdraw
    
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
    logging file debug ../binTmp/zzz48r1-log.run
    !
    vrf definition tester
     exit
    !
    vrf definition v1
     rd 1:1
     exit
    !
    router rip4 1
     vrf v1
     exit
    !
    router rip6 1
     vrf v1
     exit
    !
    interface loopback0
     no description
     vrf forwarding v1
     ipv4 address 2.2.2.1 255.255.255.255
     ipv6 address 4321::1 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff
     router rip4 1 enable
     router rip6 1 enable
     no shutdown
     no log-link-change
     exit
    !
    interface ethernet1
     no description
     vrf forwarding v1
     ipv4 address 1.1.1.1 255.255.255.252
     ipv6 address 1234:1::1 ffff:ffff::
     router rip4 1 enable
     router rip4 1 update-timer 1000
     router rip4 1 hold-time 4000
     router rip4 1 flush-time 4000
     router rip6 1 enable
     router rip6 1 update-timer 1000
     router rip6 1 hold-time 4000
     router rip6 1 flush-time 4000
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
    logging file debug ../binTmp/zzz48r2-log.run
    !
    vrf definition tester
     exit
    !
    vrf definition v1
     rd 1:1
     exit
    !
    router rip4 1
     vrf v1
     exit
    !
    router rip6 1
     vrf v1
     exit
    !
    interface loopback0
     no description
     vrf forwarding v1
     ipv4 address 2.2.2.2 255.255.255.255
     ipv6 address 4321::2 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff
     router rip4 1 enable
     router rip6 1 enable
     no shutdown
     no log-link-change
     exit
    !
    interface ethernet1
     no description
     vrf forwarding v1
     ipv4 address 1.1.1.2 255.255.255.252
     ipv6 address 1234:1::2 ffff:ffff::
     router rip4 1 enable
     router rip4 1 update-timer 1000
     router rip4 1 hold-time 4000
     router rip4 1 flush-time 4000
     router rip6 1 enable
     router rip6 1 update-timer 1000
     router rip6 1 hold-time 4000
     router rip6 1 flush-time 4000
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
    r2#show ipv4 rip 1 sum
    r2#show ipv4 rip 1 sum
     |~~~~~~~~~~~|~~~~~~~|~~~~~~~~~~|~~~~~~~~~~|
     | interface | learn | neighbor | uptime   |
     |-----------|-------|----------|----------|
     | ethernet1 | 1     | 1.1.1.1  | 00:00:38 |
     |___________|_______|__________|__________|
    r2#
    r2#
    ```
    
    ```
    r2#
    r2#
    r2#show ipv6 rip 1 sum
    r2#show ipv6 rip 1 sum
     |~~~~~~~~~~~|~~~~~~~|~~~~~~~~~~~|~~~~~~~~~~|
     | interface | learn | neighbor  | uptime   |
     |-----------|-------|-----------|----------|
     | ethernet1 | 1     | 1234:1::1 | 00:00:38 |
     |___________|_______|___________|__________|
    r2#
    r2#
    ```
    
    ```
    r2#
    r2#
    r2#show ipv4 rip 1 dat
    r2#show ipv4 rip 1 dat
     |~~~~~|~~~~~~~~~~~~|~~~~~~~~|~~~~~~~~~~~|~~~~~~~~~|~~~~~~~~~~|
     | typ | prefix     | metric | iface     | hop     | time     |
     |-----|------------|--------|-----------|---------|----------|
     | R   | 1.1.1.0/30 | 1/0    | ethernet1 | null    | 00:00:39 |
     | R   | 2.2.2.1/32 | 120/1  | ethernet1 | 1.1.1.1 | 00:00:00 |
     | R   | 2.2.2.2/32 | 1/0    | loopback0 | null    | 00:00:39 |
     |_____|____________|________|___________|_________|__________|
    r2#
    r2#
    ```
    
    ```
    r2#
    r2#
    r2#show ipv6 rip 1 dat
    r2#show ipv6 rip 1 dat
     |~~~~~|~~~~~~~~~~~~~|~~~~~~~~|~~~~~~~~~~~|~~~~~~~~~~~|~~~~~~~~~~|
     | typ | prefix      | metric | iface     | hop       | time     |
     |-----|-------------|--------|-----------|-----------|----------|
     | R   | 1234:1::/32 | 1/0    | ethernet1 | null      | 00:00:39 |
     | R   | 4321::1/128 | 120/1  | ethernet1 | 1234:1::1 | 00:00:01 |
     | R   | 4321::2/128 | 1/0    | loopback0 | null      | 00:00:39 |
     |_____|_____________|________|___________|___________|__________|
    r2#
    r2#
    ```
    
    ```
    r2#
    r2#
    r2#show ipv4 route v1
    r2#show ipv4 route v1
     |~~~~~|~~~~~~~~~~~~|~~~~~~~~|~~~~~~~~~~~|~~~~~~~~~|~~~~~~~~~~|
     | typ | prefix     | metric | iface     | hop     | time     |
     |-----|------------|--------|-----------|---------|----------|
     | C   | 1.1.1.0/30 | 0/0    | ethernet1 | null    | 00:00:39 |
     | LOC | 1.1.1.2/32 | 0/1    | ethernet1 | null    | 00:00:39 |
     | R   | 2.2.2.1/32 | 120/1  | ethernet1 | 1.1.1.1 | 00:00:01 |
     | C   | 2.2.2.2/32 | 0/0    | loopback0 | null    | 00:00:39 |
     |_____|____________|________|___________|_________|__________|
    r2#
    r2#
    ```
    
    ```
    r2#
    r2#
    r2#show ipv6 route v1
    r2#show ipv6 route v1
     |~~~~~|~~~~~~~~~~~~~~~|~~~~~~~~|~~~~~~~~~~~|~~~~~~~~~~~|~~~~~~~~~~|
     | typ | prefix        | metric | iface     | hop       | time     |
     |-----|---------------|--------|-----------|-----------|----------|
     | C   | 1234:1::/32   | 0/0    | ethernet1 | null      | 00:00:40 |
     | LOC | 1234:1::2/128 | 0/1    | ethernet1 | null      | 00:00:40 |
     | R   | 4321::1/128   | 120/1  | ethernet1 | 1234:1::1 | 00:00:02 |
     | C   | 4321::2/128   | 0/0    | loopback0 | null      | 00:00:40 |
     |_____|_______________|________|___________|___________|__________|
    r2#
    r2#
    ```
