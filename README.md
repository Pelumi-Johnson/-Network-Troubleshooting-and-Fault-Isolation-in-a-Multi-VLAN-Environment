# Network Troubleshooting and Fault Isolation in a Multi VLAN Environment

## Overview
Simulated multiple network failures in a segmented router on a stick environment and performed structured troubleshooting to isolate root causes and restore connectivity. Validated fault detection across VLAN assignment, IP addressing, trunking, default gateway configuration, and router encapsulation.

## Objective
Introduced controlled network faults and applied step by step troubleshooting methods to identify failure points, correct misconfigurations, and restore end to end communication across VLAN segments.

## Network Setup
Devices Used
- 1 Cisco 2960 Switch
- 1 Cisco 1941 Router
- Assigned 2 End Devices to each VLAN

Topology Design
```
VLAN 10 HR
PC0 HR
PC1 HR 2

VLAN 20 IT
PC2 IT
PC3 IT 2

VLAN 30 Finance
PC4 Finance
PC5 Finance 2
```
Switch connected to router through trunk link FastEthernet0/24 to GigabitEthernet0/0

Router configured with subinterfaces for inter VLAN routing

ACLs present from prior configuration

Topology
![Network Topology](https://github.com/Pelumi-Johnson/-Network-Troubleshooting-and-Fault-Isolation-in-a-Multi-VLAN-Environment/blob/main/Screenshot%202026-04-10%20144822.png)

## Configuration Baseline
Verified baseline operation before fault introduction

Confirmed VLAN assignment trunk status router subinterfaces IP addressing default gateways and ACL behavior were functioning as expected

Baseline Validation
![Baseline Validation](https://github.com/Pelumi-Johnson/-Network-Troubleshooting-and-Fault-Isolation-in-a-Multi-VLAN-Environment/blob/main/Screenshot%202026-04-09%20164953.png)
![Baseline Validation](https://github.com/Pelumi-Johnson/-Network-Troubleshooting-and-Fault-Isolation-in-a-Multi-VLAN-Environment/blob/main/Screenshot%202026-04-09%20164953.png)

## Fault Injection

### Scenario 1 Wrong VLAN Assignment
Moved PC0 to an incorrect VLAN causing loss of expected network membership and communication failure

Screenshot Placeholder Wrong VLAN Assignment
![Wrong VLAN Assignment](./screenshots/wrong-vlan.png)

### Scenario 2 Wrong IP Address
Changed PC2 addressing to an invalid network value

192.168.50.10

Result
PC2 could no longer communicate with intended VLAN segments due to subnet mismatch

Screenshot Placeholder Wrong IP
![Wrong IP](./screenshots/wrong-ip.png)

### Scenario 3 Trunk Removal
Removed trunk configuration from the switch uplink preventing VLAN tagged traffic from reaching the router

enable
configure terminal
interface fastEthernet 0/24
no switchport mode trunk
exit

Screenshot Placeholder Trunk Failure
![Trunk Failure](./screenshots/trunk-failure.png)

### Scenario 4 Wrong Default Gateway
Configured an incorrect gateway on the end device preventing traffic from being routed outside the local subnet

Screenshot Placeholder Wrong Gateway
![Wrong Gateway](./screenshots/wrong-gateway.png)

### Scenario 5 Missing dot1Q Encapsulation
Removed VLAN encapsulation from a router subinterface preventing VLAN traffic from being identified correctly

Screenshot Placeholder Missing dot1Q
![Missing dot1Q](./screenshots/missing-dot1q.png)

## Troubleshooting Process

### Initial Connectivity Testing
Used ping testing to verify communication failure and establish symptom scope

ping 192.168.10.10
ping 192.168.20.10
ping 192.168.30.10

Screenshot Placeholder Ping Failure
![Ping Failure](./screenshots/ping-failure.png)

### VLAN Verification
Checked switch VLAN membership to confirm ports were assigned to correct VLANs

show vlan brief

Screenshot Placeholder VLAN Check
![Show VLAN Brief](./screenshots/show-vlan-brief.png)

### Trunk Verification
Inspected trunk link status to verify VLAN traffic was permitted between switch and router

show interfaces trunk

Screenshot Placeholder Trunk Check
![Show Interfaces Trunk](./screenshots/show-interfaces-trunk.png)

### Router Interface Verification
Verified router physical and subinterface status to confirm routed paths were operational

show ip interface brief

Screenshot Placeholder Router Check
![Show IP Interface Brief](./screenshots/show-ip-interface-brief.png)

### ACL Verification
Reviewed ACL entries to confirm traffic filtering behavior was not the cause of unintended failures

show access-lists

Screenshot Placeholder ACL Check
![Show Access Lists](./screenshots/show-access-lists.png)

### Path Based Diagnosis
Traced failures across the communication path using a structured device to device approach

PC to Switch to Router to Destination

Result
Faults were isolated based on where communication stopped rather than through guesswork

## Remediation

### Fix 1 Corrected VLAN Assignment
Returned PC0 interface to the intended VLAN restoring proper Layer 2 membership

Screenshot Placeholder VLAN Fix
![VLAN Fix](./screenshots/vlan-fix.png)

### Fix 2 Corrected IP Addressing
Reconfigured PC2 with a valid IP address matching the VLAN 30 subnet

Example
IP 192.168.30.10
Subnet Mask 255.255.255.0
Default Gateway 192.168.30.1

Screenshot Placeholder IP Fix
![IP Fix](./screenshots/ip-fix.png)

### Fix 3 Restored Trunk Link
Re enabled trunking on the switch uplink to resume transport of multiple VLANs

enable
configure terminal
interface fastEthernet 0/24
switchport mode trunk
exit

Screenshot Placeholder Trunk Fix
![Trunk Fix](./screenshots/trunk-fix.png)

### Fix 4 Corrected Default Gateway
Updated the affected host with the correct gateway to restore routed communication

Screenshot Placeholder Gateway Fix
![Gateway Fix](./screenshots/gateway-fix.png)

### Fix 5 Reapplied dot1Q Encapsulation
Restored VLAN tagging on the router subinterface to correctly process VLAN traffic

enable
configure terminal
interface gigabitEthernet 0/0.10
encapsulation dot1Q 10
exit

Screenshot Placeholder dot1Q Fix
![dot1Q Fix](./screenshots/dot1q-fix.png)

## Validation
Retested connectivity after each correction to confirm restoration of expected communication paths

ping 192.168.10.10
ping 192.168.20.10
ping 192.168.30.10

Result
Connectivity restored across all intended VLAN paths following correction of each fault

Screenshot Placeholder Final Ping Test
![Final Ping Test](./screenshots/final-ping-test.png)

## Key Concepts Applied
Structured troubleshooting using symptom based isolation
VLAN membership verification and port assignment control
IP subnet and default gateway validation
802.1Q trunk dependency for router on a stick operation
Router subinterface verification and encapsulation integrity
ACL review as part of layered troubleshooting
Path based fault isolation from endpoint to destination

## Outcome
Executed controlled fault simulation and restoration across a multi VLAN network, demonstrating the ability to isolate and resolve configuration failures using a repeatable troubleshooting process. Delivered a validated network recovery workflow aligned with real world operational support and network engineering practice.
