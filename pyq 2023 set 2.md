# pyq some questions from 2023 set 2

## Question (3)

### (a)
To configure VLANs 10, 20, and 30 on switch SW2 and enable inter-VLAN communication through a single interface on router R2 in Cisco Packet Tracer, follow these steps. This configuration will use a router-on-a-stick setup, where a single physical interface on the router is divided into sub-interfaces for each VLAN.

### Configuration on Switch SW2

1. **Enter Global Configuration Mode**:
   ```plaintext
   SW2> enable
   SW2# configure terminal
   ```

2. **Create VLANs 10, 20, and 30**:
   ```plaintext
   SW2(config)# vlan 10
   SW2(config-vlan)# name VLAN10
   SW2(config-vlan)# exit

   SW2(config)# vlan 20
   SW2(config-vlan)# name VLAN20
   SW2(config-vlan)# exit

   SW2(config)# vlan 30
   SW2(config-vlan)# name VLAN30
   SW2(config-vlan)# exit
   ```

3. **Assign VLANs to Ports** (Example: Assigning VLAN 10 to FastEthernet 0/1, VLAN 20 to FastEthernet 0/2, and VLAN 30 to FastEthernet 0/3):
   ```plaintext
   SW2(config)# interface range fa0/1
   SW2(config-if-range)# switchport mode access
   SW2(config-if-range)# switchport access vlan 10
   SW2(config-if-range)# exit

   SW2(config)# interface range fa0/2
   SW2(config-if-range)# switchport mode access
   SW2(config-if-range)# switchport access vlan 20
   SW2(config-if-range)# exit

   SW2(config)# interface range fa0/3
   SW2(config-if-range)# switchport mode access
   SW2(config-if-range)# switchport access vlan 30
   SW2(config-if-range)# exit
   ```

4. **Configure Trunk Port** (Example: Trunking on port FastEthernet 0/24 connected to router R2):
   ```plaintext
   SW2(config)# interface fa0/24
   SW2(config-if)# switchport mode trunk
   SW2(config-if)# switchport trunk encapsulation dot1q
   SW2(config-if)# exit
   ```

### Configuration on Router R2

1. **Enter Global Configuration Mode**:
   ```plaintext
   R2> enable
   R2# configure terminal
   ```

2. **Configure Sub-interfaces for Each VLAN**:
   ```plaintext
   R2(config)# interface fa0/0.10
   R2(config-subif)# encapsulation dot1q 10
   R2(config-subif)# ip address 192.168.10.1 255.255.255.0
   R2(config-subif)# exit

   R2(config)# interface fa0/0.20
   R2(config-subif)# encapsulation dot1q 20
   R2(config-subif)# ip address 192.168.20.1 255.255.255.0
   R2(config-subif)# exit

   R2(config)# interface fa0/0.30
   R2(config-subif)# encapsulation dot1q 30
   R2(config-subif)# ip address 192.168.30.1 255.255.255.0
   R2(config-subif)# exit
   ```

3. **Ensure the Physical Interface is Up**:
   ```plaintext
   R2(config)# interface fa0/0
   R2(config-if)# no shutdown
   R2(config-if)# exit
   ```

### Verification Commands

1. **Verify VLANs on Switch SW2**:
   ```plaintext
   SW2# show vlan brief
   ```
   Expected output:
   ```plaintext
   VLAN Name                             Status    Ports
   ---- -------------------------------- --------- -------------------------------
   1    default                          active    Fa0/4, Fa0/5, Fa0/6, Fa0/7, Fa0/8
                                                 ...
   10   VLAN10                           active    Fa0/1
   20   VLAN20                           active    Fa0/2
   30   VLAN30                           active    Fa0/3
   ```

2. **Verify Trunk Ports on Switch SW2**:
   ```plaintext
   SW2# show interfaces trunk
   ```
   Expected output:
   ```plaintext
   Port      Mode         Encapsulation  Status        Native vlan
   Fa0/24    on           802.1q         trunking      1

   Port      Vlans allowed on trunk
   Fa0/24    1-4094

   Port      Vlans allowed and active in management domain
   Fa0/24    1,10,20,30

   Port      Vlans in spanning tree forwarding state and not pruned
   Fa0/24    1,10,20,30
   ```

3. **Verify Sub-interfaces on Router R2**:
   ```plaintext
   R2# show ip interface brief
   ```
   Expected output:
   ```plaintext
   Interface              IP-Address      OK? Method Status                Protocol
   FastEthernet0/0        unassigned      YES unset  up                    up
   FastEthernet0/0.10     192.168.10.1    YES manual up                    up
   FastEthernet0/0.20     192.168.20.1    YES manual up                    up
   FastEthernet0/0.30     192.168.30.1    YES manual up                    up
   ```
### (b)
To enable inter-VLAN communication through access mode, typically you would need to use a different approach than the traditional router-on-a-stick setup, as access mode is designed for single VLAN assignments to switch ports. However, assuming the task requires configuring inter-VLAN routing while keeping some devices connected through access mode ports, here are the commands you would need.

### Configuration Commands for Inter-VLAN Routing

#### On Switch SW2
1. **Create VLANs 10, 20, and 30**:
   ```plaintext
   SW2> enable
   SW2# configure terminal
   SW2(config)# vlan 10
   SW2(config-vlan)# name VLAN10
   SW2(config-vlan)# exit

   SW2(config)# vlan 20
   SW2(config-vlan)# name VLAN20
   SW2(config-vlan)# exit

   SW2(config)# vlan 30
   SW2(config-vlan)# name VLAN30
   SW2(config-vlan)# exit
   ```

2. **Assign VLANs to Access Ports**:
   ```plaintext
   SW2(config)# interface range fa0/1
   SW2(config-if-range)# switchport mode access
   SW2(config-if-range)# switchport access vlan 10
   SW2(config-if-range)# exit

   SW2(config)# interface range fa0/2
   SW2(config-if-range)# switchport mode access
   SW2(config-if-range)# switchport access vlan 20
   SW2(config-if-range)# exit

   SW2(config)# interface range fa0/3
   SW2(config-if-range)# switchport mode access
   SW2(config-if-range)# switchport access vlan 30
   SW2(config-if-range)# exit
   ```

3. **Configure Trunk Port to Router**:
   ```plaintext
   SW2(config)# interface fa0/24
   SW2(config-if)# switchport mode trunk
   SW2(config-if)# switchport trunk encapsulation dot1q
   SW2(config-if)# exit
   ```

#### On Router R2
1. **Enter Global Configuration Mode**:
   ```plaintext
   R2> enable
   R2# configure terminal
   ```

2. **Configure Sub-interfaces for Each VLAN**:
   ```plaintext
   R2(config)# interface fa0/0.10
   R2(config-subif)# encapsulation dot1q 10
   R2(config-subif)# ip address 192.168.10.1 255.255.255.0
   R2(config-subif)# exit

   R2(config)# interface fa0/0.20
   R2(config-subif)# encapsulation dot1q 20
   R2(config-subif)# ip address 192.168.20.1 255.255.255.0
   R2(config-subif)# exit

   R2(config)# interface fa0/0.30
   R2(config-subif)# encapsulation dot1q 30
   R2(config-subif)# ip address 192.168.30.1 255.255.255.0
   R2(config-subif)# exit
   ```

3. **Ensure the Physical Interface is Up**:
   ```plaintext
   R2(config)# interface fa0/0
   R2(config-if)# no shutdown
   R2(config-if)# exit
   ```

### Troubleshooting VLAN Configuration

1. **Verify VLANs on Switch SW2**:
   ```plaintext
   SW2# show vlan brief
   ```
   This command will list all the VLANs created on the switch and their assigned ports. Make sure VLANs 10, 20, and 30 are present and ports are correctly assigned.

2. **Verify Trunk Port Configuration on SW2**:
   ```plaintext
   SW2# show interfaces trunk
   ```
   This command will show the trunking status of the interfaces, including which VLANs are allowed and active on the trunk link.

3. **Verify Sub-interfaces on Router R2**:
   ```plaintext
   R2# show ip interface brief
   ```
   This command provides a summary of the router's interfaces, including sub-interfaces, and their IP addresses and status.

4. **Check VLAN Configuration on Specific Ports**:
   ```plaintext
   SW2# show running-config interface fa0/1
   SW2# show running-config interface fa0/2
   SW2# show running-config interface fa0/3
   ```
   This command will display the running configuration of specific interfaces, ensuring they are correctly set to access mode and assigned to the right VLANs.

5. **Verify Connectivity**:
   - **Ping Between VLANs**: From a device in VLAN 10, try to ping the default gateway of VLAN 20 and VLAN 30 (i.e., `192.168.20.1` and `192.168.30.1`).
   - **Ping the Router Sub-interfaces**: Ensure that devices in each VLAN can reach their respective gateway IP addresses on the router.

6. **Check DHCP (if configured)**:
   ```plaintext
   SW2# show ip dhcp binding
   ```
   This command shows the current DHCP bindings, ensuring devices are receiving IP addresses correctly.

By following these commands, you should be able to configure and verify inter-VLAN routing on your network, ensuring proper communication between devices on different VLANs through a single interface on the router.

## Question (4) (a)

Certainly! Let's configure port security on interface i7 of switch SW2 based on your requirements. Here are the steps:

1. **Connect to Switch SW2**:

   - Access the switch via console or SSH.

   ```shell
   ssh admin@<SW2_IP_Address>
   ```

2. **Check Default Port Security Settings**:

   ```
   SW2# show port-security interface i7
   ```

3. **Configure Port Security**:

   ```
   SW2(config)# interface i7
   SW2(config-if)# switchport port-security
   SW2(config-if)# switchport port-security maximum 2
   SW2(config-if)# switchport port-security violation restrict
   ```

4. **Copy Running Config to NVRAM**:
   ```
   SW2# copy running-config startup-config
   ```

### (b)

#### (i) **Verifying Port Security Configuration:**  
To verify the configuration, use the following commands:

- To display port security details for interface i7:
  ```
  SW2# show port-security interface i7
  ```
  Look for the "Maximum MAC Addresses" and "Total MAC Addresses" fields to ensure the correct settings.
- To view the forward-filter table (list of allowed MAC addresses):
  ```
  SW2# show port-security address
  ```
- Display Port Security

  To see the summary of port security settings across all ports, you can use:

  ```
  SW2# show port-security
  ```

- **Display Running Configuration for Interface i7:**
  ```
  show running-config interface i7
  ```
#### (ii) 

1. **Viewing the Current Configuration Register Value:**
   To display the current configuration register value, you can use the following command:
   ```
   Router# show version
   ```
   Look for the configuration register setting in the last line of the output. The default value is typically shown as hexadecimal `0x2102`.

2. **Default Configuration Register Value:**
   The factory default setting for the configuration register is `0x2102`. This default value indicates that the router should:
   - Attempt to load a Cisco IOS Software image from Flash memory during boot.
   - Load the startup configuration.
   - Use a console speed of 9600 baud.

3. **Bypassing NVRAM (Ignore Startup Configuration) During Boot:**  
   To bypass loading the startup configuration from NVRAM during boot, you can set the configuration register to `0x2142`. This allows you to enter privileged EXEC mode without loading the startup configuration:
   ```
   Router(config)# config-register 0x2142
   ```

