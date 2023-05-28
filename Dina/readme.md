# Dina Walkthrough

Use netdiscover to find the ip of the vm

Command:

      $netdiscover
      
Output:

    Currently scanning: 192.168.52.0/16   |   Screen View: Unique Hosts                                              

     2 Captured ARP Req/Rep packets, from 2 hosts.   Total size: 84                                                   
     _____________________________________________________________________________
       IP            At MAC Address     Count     Len  MAC Vendor / Hostname      
     -----------------------------------------------------------------------------
     192.168.42.54   08:00:27:84:8e:f5      1      42  PCS Systemtechnik GmbH                                         
     192.168.42.129  06:23:0e:e5:82:1a      1      42  Unknown vendor
     
We got the ip.addr==192.168.42.54
