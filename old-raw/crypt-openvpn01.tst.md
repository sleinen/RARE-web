# Example: openvpn over ipv4
    
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
    logging file debug ../binTmp/zzz4r1-log.run
    !
    crypto ipsec ips
     cipher des
     hash md5
     key $v10$MjJmOWM2NzZmNjU1MzM2YzNmMzE4OGI4ZDljYzc1OTkwMzczMzIxMmVkNzcyMzFiYzM4MTI2YjYwMDBiMDQzZjFmNTZkMDdiODg1ZjRkMDA2NzZhZmQ4ZmVhMjVjODhmYTkxNzI5NGQ4ZjFlODliODQ5MjJkNWQyNTU2ZGU5NzdiZWFjMmYyNTRiYTJiNjc0NzcxMzFmNGQ0NzA4Y2I1MDlmNGM5Zjc4NDc4MDQ2NTQ2MmU1MDJkMjkxODM2NjViYmQ1ZWZmNmJkYzI3MzcwZjA1YWExZDg1NmI0OTdhMWY3ZWY1ZjIwYmFkN2FmZjE1NTYxOWE0YjA5ODQ5ZmFiODE0ZWU3NmU3MTIxYzJhZGY4NTMyNmRiNGMxY2NlMTMyMjAwY2EzZTRkMDM5MzBmNzY1YmE5NmE4YzQ2ZjFhYjM3NGJlYjczZTc5MDkzZDYwODc5YThkOTU4NWYyZmViOTg3ZDg5ZTY1YTMzZWYzODU3ZjNiMDlkZjgwYTI0MDNmNmM1MGRjNTA0MzllMjU4ZDYxYzdkYWMzNzc1MTRhOGQyODFjMTBmZWVlYTc5YWU3YjA2MzA2NGFlYzM5ODliNGQ4NjdiYjI0MTgyZjdkMDA3YWQ0MTI4NGVlNjU3NzA1M2RhZTJjYzI4OWRkMzllNjZjZDhmZTcwODliNzAxNWY=
     exit
    !
    vrf definition tester
     exit
    !
    vrf definition v1
     rd 1:1
     exit
    !
    interface serial1
     no description
     encapsulation hdlc
     vrf forwarding v1
     ipv4 address 1.1.1.1 255.255.255.0
     ipv6 address 1234::1 ffff::
     no shutdown
     no log-link-change
     exit
    !
    interface tunnel1
     no description
     tunnel vrf v1
     tunnel protection ips
     tunnel source serial1
     tunnel destination 1.1.1.2
     tunnel mode openvpn
     vrf forwarding v1
     ipv4 address 2.2.2.1 255.255.255.0
     ipv6 address 4321::1 ffff::
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
    logging file debug ../binTmp/zzz4r2-log.run
    !
    crypto ipsec ips
     cipher des
     hash md5
     key $v10$MjJmOWM2NzZmNjU1MzM2YzNmMzE4OGI4ZDljYzc1OTkwMzczMzIxMmVkNzcyMzFiYzM4MTI2YjYwMDBiMDQzZjFmNTZkMDdiODg1ZjRkMDA2NzZhZmQ4ZmVhMjVjODhmYTkxNzI5NGQ4ZjFlODliODQ5MjJkNWQyNTU2ZGU5NzdiZWFjMmYyNTRiYTJiNjc0NzcxMzFmNGQ0NzA4Y2I1MDlmNGM5Zjc4NDc4MDQ2NTQ2MmU1MDJkMjkxODM2NjViYmQ1ZWZmNmJkYzI3MzcwZjA1YWExZDg1NmI0OTdhMWY3ZWY1ZjIwYmFkN2FmZjE1NTYxOWE0YjA5ODQ5ZmFiODE0ZWU3NmU3MTIxYzJhZGY4NTMyNmRiNGMxY2NlMTMyMjAwY2EzZTRkMDM5MzBmNzY1YmE5NmE4YzQ2ZjFhYjM3NGJlYjczZTc5MDkzZDYwODc5YThkOTU4NWYyZmViOTg3ZDg5ZTY1YTMzZWYzODU3ZjNiMDlkZjgwYTI0MDNmNmM1MGRjNTA0MzllMjU4ZDYxYzdkYWMzNzc1MTRhOGQyODFjMTBmZWVlYTc5YWU3YjA2MzA2NGFlYzM5ODliNGQ4NjdiYjI0MTgyZjdkMDA3YWQ0MTI4NGVlNjU3NzA1M2RhZTJjYzI4OWRkMzllNjZjZDhmZTcwODliNzAxNWY=
     exit
    !
    vrf definition tester
     exit
    !
    vrf definition v1
     rd 1:1
     exit
    !
    interface serial1
     no description
     encapsulation hdlc
     vrf forwarding v1
     ipv4 address 1.1.1.2 255.255.255.0
     ipv6 address 1234::2 ffff::
     no shutdown
     no log-link-change
     exit
    !
    interface tunnel1
     no description
     tunnel vrf v1
     tunnel protection ips
     tunnel source serial1
     tunnel destination 1.1.1.1
     tunnel mode openvpn
     vrf forwarding v1
     ipv4 address 2.2.2.2 255.255.255.0
     ipv6 address 4321::2 ffff::
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
    r1#
    r1#
    r1#show inter tun1 full
    r1#show inter tun1 full
    tunnel1 is up (since 00:00:05, 15 changes)
     description:
     type is openvpn, hwaddr=none, mtu=1400, bw=8000kbps, vrf=v1
     ip4 address=2.2.2.1/24, netmask=255.255.255.0, ifcid=315376672
     ip6 address=4321::1/16, netmask=ffff::, ifcid=602333394
     received 22 packets (1628 bytes) dropped 0 packets (0 bytes)
     transmitted 28 packets (1880 bytes) promisc=false macsec=false
     |~~~~~~~|~~~~|~~~~|~~~~~~|~~~~~|~~~~~|~~~~~~|
     |       | packet         | byte             |
     | time  | tx | rx | drop | tx  | rx  | drop |
     |-------|----|----|------|-----|-----|------|
     | 1sec  | 2  | 2  | 0    | 132 | 148 | 0    |
     | 1min  | 0  | 0  | 0    | 0   | 0   | 0    |
     | 1hour | 0  | 0  | 0    | 0   | 0   | 0    |
     |_______|____|____|______|_____|_____|______|
     |~~~~~~~~|~~~~~~~|~~~~~~~~~|~~~~|~~~~|~~~~~~|~~~~~|~~~~~|~~~~~~|
     |                          | packet         | byte             |
     | type   | value | handler | tx | rx | drop | tx  | rx  | drop |
     |--------|-------|---------|----|----|------|-----|-----|------|
     | ethtyp | 0000  | null    | 0  | 0  | 0    | 0   | 0   | 0    |
     | ethtyp | 0800  | ip4     | 15 | 12 | 0    | 990 | 888 | 0    |
     | ethtyp | 86dd  | ip6     | 13 | 10 | 0    | 890 | 740 | 0    |
     |________|_______|_________|____|____|______|_____|_____|______|
     |~~~~~|~~~~|~~~~|
     | who | tx | rx |
     |-----|----|----|
     |_____|____|____|
     |~~~~~~~|~~~~~~|~~~~~~|
     | proto | pack | byte |
     |-------|------|------|
     | 1     | 15   | 990  |
     | 58    | 13   | 890  |
     |_______|______|______|
     |~~~~~~~~~~~~|~~~~|~~~~|~~~~~~|~~~~~~|~~~~~~|~~~~~~|
     |            | packet         | byte               |
     | size       | tx | rx | drop | tx   | rx   | drop |
     |------------|----|----|------|------|------|------|
     | 0-255      | 28 | 22 | 0    | 1880 | 1628 | 0    |
     | 256-511    | 0  | 0  | 0    | 0    | 0    | 0    |
     | 512-767    | 0  | 0  | 0    | 0    | 0    | 0    |
     | 768-1023   | 0  | 0  | 0    | 0    | 0    | 0    |
     | 1024-1279  | 0  | 0  | 0    | 0    | 0    | 0    |
     | 1280-1535  | 0  | 0  | 0    | 0    | 0    | 0    |
     | 1536-1791  | 0  | 0  | 0    | 0    | 0    | 0    |
     | 1792-65535 | 0  | 0  | 0    | 0    | 0    | 0    |
     |____________|____|____|______|______|______|______|
     |~~~~~~~|~~~~~|~~~~~|~~~~~~|~~~~~~|~~~~~~|~~~~~~|
     |       | packet           | byte               |
     | class | cos | exp | prec | cos  | exp  | prec |
     |-------|-----|-----|------|------|------|------|
     | 0     | 28  | 28  | 28   | 1880 | 1880 | 1880 |
     | 1     | 0   | 0   | 0    | 0    | 0    | 0    |
     | 2     | 0   | 0   | 0    | 0    | 0    | 0    |
     | 3     | 0   | 0   | 0    | 0    | 0    | 0    |
     | 4     | 0   | 0   | 0    | 0    | 0    | 0    |
     | 5     | 0   | 0   | 0    | 0    | 0    | 0    |
     | 6     | 0   | 0   | 0    | 0    | 0    | 0    |
     | 7     | 0   | 0   | 0    | 0    | 0    | 0    |
     |_______|_____|_____|______|______|______|______|
            2240|
            2016|#
            1792|#   #
            1568|#   #
            1344|#   #
            1120|#   #
             896|#   #
             672|#   #
             448|#####
             224|#####
               0|#####
             bps|0---------10--------20--------30--------40--------50-------- seconds
               1|
               0|
               0|
               0|
               0|
               0|
               0|
               0|
               0|
               0|
               0|
             bps|0---------10--------20--------30--------40--------50-------- minutes
               1|
               0|
               0|
               0|
               0|
               0|
               0|
               0|
               0|
               0|
               0|
             bps|0---------10--------20--------30--------40--------50-------- hours
    r1#
    r1#
    ```
