# Example: fragmentation and reassembly
    
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
    logging file debug ../binTmp/zzz43r1-log.run
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
     mtu 1500
     encapsulation hdlc
     enforce-mtu both
     vrf forwarding v1
     ipv4 address 1.1.1.1 255.255.255.0
     ipv4 reassembly 16
     ipv4 fragmentation 1400
     ipv6 address 1234::1 ffff::
     ipv6 reassembly 16
     ipv6 fragmentation 1400
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
    logging file debug ../binTmp/zzz43r2-log.run
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
     mtu 1500
     encapsulation hdlc
     enforce-mtu both
     vrf forwarding v1
     ipv4 address 1.1.1.2 255.255.255.0
     ipv4 reassembly 16
     ipv4 fragmentation 1400
     ipv6 address 1234::2 ffff::
     ipv6 reassembly 16
     ipv6 fragmentation 1400
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
