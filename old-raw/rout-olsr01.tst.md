# Example: olsr on one subnet
    
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
    logging file debug ../binTmp/zzz44r1-log.run
    !
    vrf definition tester
     exit
    !
    vrf definition v1
     rd 1:1
     exit
    !
    router olsr4 1
     vrf v1
     redistribute connected
     exit
    !
    router olsr6 1
     vrf v1
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
    interface ethernet1
     no description
     vrf forwarding v1
     ipv4 address 1.1.1.1 255.255.255.0
     ipv6 address 1234::1 ffff::
     router olsr4 1 enable
     router olsr6 1 enable
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
    logging file debug ../binTmp/zzz44r2-log.run
    !
    bridge 1
     mac-learn
     exit
    !
    vrf definition tester
     exit
    !
    vrf definition v1
     rd 1:1
     exit
    !
    router olsr4 1
     vrf v1
     redistribute connected
     exit
    !
    router olsr6 1
     vrf v1
     redistribute connected
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
    interface bvi1
     no description
     vrf forwarding v1
     ipv4 address 1.1.1.2 255.255.255.0
     ipv6 address 1234::2 ffff::
     router olsr4 1 enable
     router olsr6 1 enable
     no shutdown
     no log-link-change
     exit
    !
    interface ethernet1
     no description
     bridge-group 1
     no shutdown
     no log-link-change
     exit
    !
    interface ethernet2
     no description
     bridge-group 1
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
    logging file debug ../binTmp/zzz44r3-log.run
    !
    bridge 1
     mac-learn
     exit
    !
    vrf definition tester
     exit
    !
    vrf definition v1
     rd 1:1
     exit
    !
    router olsr4 1
     vrf v1
     redistribute connected
     exit
    !
    router olsr6 1
     vrf v1
     redistribute connected
     exit
    !
    interface loopback0
     no description
     vrf forwarding v1
     ipv4 address 2.2.2.3 255.255.255.255
     ipv6 address 4321::3 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff
     no shutdown
     no log-link-change
     exit
    !
    interface bvi1
     no description
     vrf forwarding v1
     ipv4 address 1.1.1.3 255.255.255.0
     ipv6 address 1234::3 ffff::
     router olsr4 1 enable
     router olsr6 1 enable
     no shutdown
     no log-link-change
     exit
    !
    interface ethernet1
     no description
     bridge-group 1
     no shutdown
     no log-link-change
     exit
    !
    interface ethernet2
     no description
     bridge-group 1
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
    
    **r4:**
    ```
    hostname r4
    buggy
    !
    logging file debug ../binTmp/zzz44r4-log.run
    !
    vrf definition tester
     exit
    !
    vrf definition v1
     rd 1:1
     exit
    !
    router olsr4 1
     vrf v1
     redistribute connected
     exit
    !
    router olsr6 1
     vrf v1
     redistribute connected
     exit
    !
    interface loopback0
     no description
     vrf forwarding v1
     ipv4 address 2.2.2.4 255.255.255.255
     ipv6 address 4321::4 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff
     no shutdown
     no log-link-change
     exit
    !
    interface ethernet1
     no description
     vrf forwarding v1
     ipv4 address 1.1.1.4 255.255.255.0
     ipv6 address 1234::4 ffff::
     router olsr4 1 enable
     router olsr6 1 enable
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
    r2#show ipv4 olsr 1 sum
    r2#show ipv4 olsr 1 sum
     |~~~~~~~~~~~|~~~~~~~|~~~~~~~~~~|~~~~~~~~~~|
     | interface | learn | neighbor | uptime   |
     |-----------|-------|----------|----------|
     | bvi1      | 2     | 1.1.1.1  | 00:00:38 |
     | bvi1      | 2     | 1.1.1.3  | 00:00:37 |
     | bvi1      | 2     | 1.1.1.4  | 00:00:38 |
     |___________|_______|__________|__________|
    r2#
    r2#
    ```
    
    ```
    r2#
    r2#
    r2#show ipv6 olsr 1 sum
    r2#show ipv6 olsr 1 sum
     |~~~~~~~~~~~|~~~~~~~|~~~~~~~~~~|~~~~~~~~~~|
     | interface | learn | neighbor | uptime   |
     |-----------|-------|----------|----------|
     | bvi1      | 2     | 1234::1  | 00:00:38 |
     | bvi1      | 2     | 1234::3  | 00:00:42 |
     | bvi1      | 2     | 1234::4  | 00:00:38 |
     |___________|_______|__________|__________|
    r2#
    r2#
    ```
    
    ```
    r2#
    r2#
    r2#show ipv4 olsr 1 dat
    r2#show ipv4 olsr 1 dat
     |~~~~~|~~~~~~~~~~~~|~~~~~~~~|~~~~~~~|~~~~~~~~~|~~~~~~~~~~|
     | typ | prefix     | metric | iface | hop     | time     |
     |-----|------------|--------|-------|---------|----------|
     | N   | 1.1.1.0/24 | 1/0    | bvi1  | null    | 00:00:43 |
     | N   | 2.2.2.1/32 | 140/1  | bvi1  | 1.1.1.1 | 00:00:13 |
     | N   | 2.2.2.3/32 | 140/1  | bvi1  | 1.1.1.3 | 00:00:12 |
     | N   | 2.2.2.4/32 | 140/1  | bvi1  | 1.1.1.4 | 00:00:13 |
     |_____|____________|________|_______|_________|__________|
    r2#
    r2#
    ```
    
    ```
    r2#
    r2#
    r2#show ipv6 olsr 1 dat
    r2#show ipv6 olsr 1 dat
     |~~~~~|~~~~~~~~~~~~~|~~~~~~~~|~~~~~~~|~~~~~~~~~|~~~~~~~~~~|
     | typ | prefix      | metric | iface | hop     | time     |
     |-----|-------------|--------|-------|---------|----------|
     | N   | 1234::/16   | 1/0    | bvi1  | null    | 00:00:43 |
     | N   | 4321::1/128 | 140/1  | bvi1  | 1234::1 | 00:00:13 |
     | N   | 4321::3/128 | 140/1  | bvi1  | 1234::3 | 00:00:43 |
     | N   | 4321::4/128 | 140/1  | bvi1  | 1234::4 | 00:00:13 |
     |_____|_____________|________|_______|_________|__________|
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
     | C   | 1.1.1.0/24 | 0/0    | bvi1      | null    | 00:00:43 |
     | LOC | 1.1.1.2/32 | 0/1    | bvi1      | null    | 00:00:43 |
     | N   | 2.2.2.1/32 | 140/1  | bvi1      | 1.1.1.1 | 00:00:13 |
     | C   | 2.2.2.2/32 | 0/0    | loopback0 | null    | 00:00:43 |
     | N   | 2.2.2.3/32 | 140/1  | bvi1      | 1.1.1.3 | 00:00:13 |
     | N   | 2.2.2.4/32 | 140/1  | bvi1      | 1.1.1.4 | 00:00:13 |
     |_____|____________|________|___________|_________|__________|
    r2#
    r2#
    ```
    
    ```
    r2#
    r2#
    r2#show ipv6 route v1
    r2#show ipv6 route v1
     |~~~~~|~~~~~~~~~~~~~|~~~~~~~~|~~~~~~~~~~~|~~~~~~~~~|~~~~~~~~~~|
     | typ | prefix      | metric | iface     | hop     | time     |
     |-----|-------------|--------|-----------|---------|----------|
     | C   | 1234::/16   | 0/0    | bvi1      | null    | 00:00:43 |
     | LOC | 1234::2/128 | 0/1    | bvi1      | null    | 00:00:43 |
     | N   | 4321::1/128 | 140/1  | bvi1      | 1234::1 | 00:00:13 |
     | C   | 4321::2/128 | 0/0    | loopback0 | null    | 00:00:43 |
     | N   | 4321::3/128 | 140/1  | bvi1      | 1234::3 | 00:00:43 |
     | N   | 4321::4/128 | 140/1  | bvi1      | 1234::4 | 00:00:13 |
     |_____|_____________|________|___________|_________|__________|
    r2#
    r2#
    ```
