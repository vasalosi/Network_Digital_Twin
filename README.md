# Network_Digital_Twin
Creating a 5G Network Digital Twin to be used for 6G development


The 5G network consists of three main components: the User Equipment (UE), the 5G New Radio (gNodeB), and the core network (5GC)

## Open5GS
The 5G Core system is implemented using the Open5GS, an open source implementation of 5G mobile core network.
 The 5G Core System consists of the following network functions(NF). These functions are divided into two main planes: the control plane and the user plane.

1. User plane Function (UPF)
2. Data network (DN), e.g. operator services, Internet access or 3rd party services
3. Core Access and Mobility Management Function (AMF)
4. Authentication Server Function (AUSF)
5. Session Management Function (SMF)
6. Network Slice Selection Function (NSSF)
7. Network Exposure Function (NEF)
7. NF Repository Function (NRF)
9. Policy Control function (PCF)
10. Unified Data Management (UDM)
11. Application Function (AF)

## UERANSIM
UERANSIM is an open source 5G UE & 5G RAN(gNodeB) implementation. It can be considered as a 5G mobile phone and a base station in basic terms. There are 3 main interface in UE/RAN perspective, 1) Control Interface (between RAN and AMF), 2) User Interface (between RAN and UPF), 3) Radio Interface (between UE and RAN)

### Open5GS Installation
```
# install open5gs as daemon service
sudo apt update
sudo apt install software-properties-common
sudo add-apt-repository ppa:open5gs/latest
sudo apt update
sudo apt install open5gs
```
Configure the NGAP bind address of the AMF(5G Core running server IP) and the GTPU bind address of the UPF(5G Core running server IP) in order for gNB and UE to be able to connect to 5G Core network. 
After doing these configurationsm restart the AMF and UPF services.
AMF(5G Core running server IP)
```diff
# amf config file locates in /etc/open5gs/amf.yaml
# ngap addr configured to 5g core server ip (open5gs VM own IP address)
amf:
  sbi:
    server:
      - address: 127.0.0.5
        port: 7777
    client:
#      nrf:
#        - uri: http://127.0.0.10:7777
      scp:
        - uri: http://127.0.0.200:7777
  ngap:
server:
+ - address: 10.160.101.188 }
+ - port: 38412


# restart amf services
sudo systemctl restart open5gs-amfd

---

# amf logs can be found in /var/log/open5gs/amf.log
sudo tail -f /var/log/open5gs/amf.log
02/12 10:49:25.206: [app] INFO: Configuration: '/etc/open5gs/amf.yaml' (../lib/app/ogs-init.c:133)
02/12 10:49:25.206: [app] INFO: File Logging: '/var/log/open5gs/amf.log' (../lib/app/ogs-init.c:136)
02/12 10:49:25.211: [sbi] INFO: NF EndPoint(addr) setup [127.0.0.200:7777] (../lib/sbi/context.c:474)
02/12 10:49:25.211: [metrics] INFO: metrics_server() [http://127.0.0.5]:9090 (../lib/metrics/prometheus/context.c:299)
02/12 10:49:25.211: [sbi] INFO: NF Service [namf-comm] (../lib/sbi/context.c:1829)
02/12 10:49:25.212: [sbi] INFO: nghttp2_server() [http://127.0.0.5]:7777 (../lib/sbi/nghttp2-server.c:424)
02/12 10:49:25.212: [amf] INFO: ngap_server() [10.160.101.188]:38412 (../src/amf/ngap-sctp.c:61)
02/12 10:49:25.213: [sctp] INFO: AMF initialize...done (../src/amf/app.c:33)
```
UPF(5G Core running server IP)
```diff
upf:
  pfcp:
    server:
      - address: 127.0.0.7
    client:
#      smf:     #  UPF PFCP Client try to associate SMF PFCP Server
#        - address: 127.0.0.4
  gtpu:
    server:
+      - address: 10.160.101.188
  session:
    - subnet: 10.45.0.0/16
      gateway: 10.45.0.1
    - subnet: 2001:db8:cafe::/48
      gateway: 2001:db8:cafe::1
  metrics:
    server:
      - address: 127.0.0.7
        port: 9090

```



