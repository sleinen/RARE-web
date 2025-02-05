# Example: empty demo network
    
=== "Topology"
    
     <div class="nextWrapper">
         <iframe src="/guides/reference/snippets/next-diagram.html" style="border:none;"></iframe>
     </div>

    
=== "Configuration"
    
    **r1:**
    ```
    hostname r1
    logging file debug ../binTmp/zzz89r1-log.run
    vrf definition tester
     exit
    server telnet tester
     security protocol telnet
     vrf tester
     exit
    int eth1
     desc r2 e1
     lldp ena
     exit
    int eth2
     desc r3 e2
     lldp ena
     exit
    ```
    
    **r2:**
    ```
    hostname r2
    logging file debug ../binTmp/zzz89r2-log.run
    vrf definition tester
     exit
    server telnet tester
     security protocol telnet
     vrf tester
     exit
    int eth1
     desc r1 e1
     lldp ena
     exit
    int eth2
     desc r4 e2
     lldp ena
     exit
    ```
    
    **r3:**
    ```
    hostname r3
    logging file debug ../binTmp/zzz89r3-log.run
    vrf definition tester
     exit
    server telnet tester
     security protocol telnet
     vrf tester
     exit
    int eth1
     desc r4 e1
     lldp ena
     exit
    int eth2
     desc r1 e2
     lldp ena
     exit
    ```
    
    **r4:**
    ```
    hostname r4
    logging file debug ../binTmp/zzz89r4-log.run
    vrf definition tester
     exit
    server telnet tester
     security protocol telnet
     vrf tester
     exit
    int eth1
     desc r3 e1
     lldp ena
     exit
    int eth2
     desc r2 e2
     lldp ena
     exit
    ```
