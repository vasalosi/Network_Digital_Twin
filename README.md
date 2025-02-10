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
```
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
```ruby
server:
 - address: 10.160.101.188 }
 - port: 38412 
```
```
