# **VLAN and Subinterface Configuration Lab**

## **Overview**
This project demonstrates the configuration of VLANs, trunking, and subinterfaces on Packet Tracer. The lab includes two switches (SW1 and SW2), a router (R1), and two PCs (PC1 and PC2). The goal was to establish connectivity between devices across VLANs and troubleshoot trunking and routing issues to enable inter-VLAN communication.

---

## **Network Topology**
![image](https://github.com/user-attachments/assets/8c76b250-3700-4c73-ad89-60688a017351)


### **Devices Involved**
1. **Router (R1)** - Cisco ISR4331
2. **Switch 1 (SW1)** - Cisco 2960-24TT
3. **Switch 2 (SW2)** - Cisco 2960-24TT
4. **PC1** - Connected to SW1 (VLAN 10)
5. **PC2** - Connected to SW2 (VLAN 20)

---

## **IP Addressing Scheme**

| Device       | Interface              | IP Address      | Subnet Mask     | VLAN |
|--------------|------------------------|-----------------|-----------------|------|
| **R1**       | Gig0/0/0.10 (sub-if)   | 192.168.10.1    | 255.255.255.0   | 10   |
| **R1**       | Gig0/0/0.20 (sub-if)   | 192.168.20.1    | 255.255.255.0   | 20   |
| **PC1**      | FastEthernet0          | 192.168.10.2    | 255.255.255.0   | 10   |
| **PC2**      | FastEthernet0          | 192.168.20.3    | 255.255.255.0   | 20   |
| **SW1**      | GigabitEthernet0/1,0/2 | Trunk Ports     | N/A             | 10,20|
| **SW2**      | GigabitEthernet0/1,0/2 | Trunk Ports     | N/A             | 10,20|

---

## **Configuration Steps**

### **1. VLAN Configuration on SW1 and SW2**
```plaintext
SW1(config)# vlan 10
SW1(config-vlan)# name PC-LAN
SW1(config-vlan)# vlan 20
SW1(config-vlan)# name Support

SW2(config)# vlan 10
SW2(config-vlan)# name PC-LAN
SW2(config-vlan)# vlan 20
SW2(config-vlan)# name Support
```

### **2. Trunk Port Configuration**
```plaintext
SW1(config)# interface gigabitEthernet 0/1
SW1(config-if)# switchport mode trunk
SW1(config-if)# switchport trunk allowed vlan 10,20

SW2(config)# interface gigabitEthernet 0/1
SW2(config-if)# switchport mode trunk
SW2(config-if)# switchport trunk allowed vlan 10,20
```

### **3. Subinterface Configuration on R1**
```plaintext
R1(config)# interface gigabitEthernet 0/0/0.10
R1(config-subif)# encapsulation dot1Q 10
R1(config-subif)# ip address 192.168.10.1 255.255.255.0

R1(config)# interface gigabitEthernet 0/0/0.20
R1(config-subif)# encapsulation dot1Q 20
R1(config-subif)# ip address 192.168.20.1 255.255.255.0
```

### **4. Verify VLANs and Trunk Status**
```plaintext
SW1# show vlan brief
SW1# show interfaces trunk
SW2# show vlan brief
SW2# show interfaces trunk
```

### **5. Verify Routing on R1**
```plaintext
R1# show ip route
```

---

## **Key Troubleshooting Steps**
1. **VLAN and Trunk Verification**
   - Verified that VLAN 10 and VLAN 20 were created on both SW1 and SW2.
   - Ensured trunk ports allowed VLAN 10 and VLAN 20 using `show interfaces trunk`.

2. **Subinterface IP Overlap**
   - Faced initial overlap errors when assigning IP addresses to subinterfaces.
   - Corrected this with proper `encapsulation dot1Q <vlan>` assignments.

3. **PC Connectivity**
   - Tested connectivity using `ping`:
     - PC1 successfully pinged **192.168.10.1**.
     - PC2 successfully pinged **192.168.20.1**.
     - Verified inter-VLAN routing via router subinterfaces.

4. **Interface Issues**
   - Identified and resolved issues with **GigabitEthernet 0/1** and **GigabitEthernet 0/2** on R1 and SW2.

---

## **Successful Results**
- **PC1** (VLAN 10) can ping its gateway **192.168.10.1** and reach devices in VLAN 20.
- **PC2** (VLAN 20) can ping its gateway **192.168.20.1** and reach devices in VLAN 10.
- Inter-VLAN routing works as configured via R1 subinterfaces.

---

## **Lessons Learned**
1. VLANs must be allowed on trunk ports to pass tagged traffic.
2. Subinterfaces on a router require `encapsulation dot1Q` to identify VLAN traffic.
3. Always verify IP addressing to avoid overlaps or misconfigurations.
4. Properly checking port states and trunking configurations can resolve connectivity issues.

---

## **Files Included**
- **`basicConfig.pkt`**: Packet Tracer project file with the complete network configuration.

---

## **How to Use**
1. Open the **`basicConfig.pkt`** file in Cisco Packet Tracer.
2. Verify the network topology and configurations.
3. Test connectivity using `ping` between devices.

---

## **Future Improvements**
- Implement routing protocols like **OSPF** or **EIGRP** for larger networks.
- Configure **port security** to restrict unauthorized access.
- Add **Spanning Tree Protocol (STP)** to prevent network loops.

---

This lab demonstrates practical VLAN configuration, inter-VLAN routing via router-on-a-stick, and key troubleshooting techniques.

--- 

