# pyq some questions from 2023 set 2

## Question (3)

### (a)

To configure VLANs 10, 20, and 30 on switch SW2 and enable inter-VLAN communication through a single interface on router R2 in Cisco Packet Tracer, follow these steps. This configuration will use a router-on-a-stick setup, where a single physical interface on the router is divided into sub-interfaces for each VLAN.

#### Configuration on Switch SW2

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

#### Configuration on Router R2

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

#### Verification Commands

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

#### Configuration Commands for Inter-VLAN Routing

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

#### Troubleshooting VLAN Configuration

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

## Question (5)

### (a)

To achieve the described setup, you need to configure DHCP relay (DHCP helper) on the router R2 for devices under SW3 to obtain IP addresses from R2, and ensure devices under SW4 get their IP addresses from a dedicated DHCP server. Here's the detailed configuration:

#### Configuration on Router R2 for DHCP Server

1. **Enter Global Configuration Mode**:

   ```plaintext
   R2> enable
   R2# configure terminal
   ```

2. **Configure DHCP Pool for Devices under SW3**:

   ```plaintext
   R2(config)# ip dhcp pool SW3_POOL
   R2(dhcp-config)# network 192.168.50.0 255.255.255.0
   R2(dhcp-config)# default-router 192.168.50.1
   R2(dhcp-config)# dns-server 8.8.8.8 8.8.4.4
   R2(dhcp-config)# exit
   ```

3. **Configure Interface for SW3 Network**:

   ```plaintext
   R2(config)# interface fa0/1.50
   R2(config-subif)# encapsulation dot1q 50
   R2(config-subif)# ip address 192.168.50.1 255.255.255.0
   R2(config-subif)# exit
   ```

4. **Ensure the Physical Interface is Up**:
   ```plaintext
   R2(config)# interface fa0/1
   R2(config-if)# no shutdown
   R2(config-if)# exit
   ```

#### Configuration on SW3

1. **Create and Assign VLAN 50**:

   ```plaintext
   SW3> enable
   SW3# configure terminal
   SW3(config)# vlan 50
   SW3(config-vlan)# name VLAN50
   SW3(config-vlan)# exit

   SW3(config)# interface range fa0/1 - 24
   SW3(config-if-range)# switchport mode access
   SW3(config-if-range)# switchport access vlan 50
   SW3(config-if-range)# exit
   ```

2. **Configure Trunk Port to Router R2**:
   ```plaintext
   SW3(config)# interface fa0/24
   SW3(config-if)# switchport mode trunk
   SW3(config-if)# switchport trunk encapsulation dot1q
   SW3(config-if)# exit
   ```

#### Configuration on Router R2 for DHCP Relay (SW4)

1. **Configure Interface for SW4 Network**:

   ```plaintext
   R2(config)# interface fa0/1.60
   R2(config-subif)# encapsulation dot1q 60
   R2(config-subif)# ip address 192.168.60.1 255.255.255.0
   R2(config-subif)# exit
   ```

2. **Configure DHCP Relay on R2**:
   ```plaintext
   R2(config)# interface fa0/1.60
   R2(config-if)# ip helper-address [DHCP_SERVER_IP]
   R2(config-if)# exit
   ```

Replace `[DHCP_SERVER_IP]` with the actual IP address of the dedicated DHCP server for SW4.

#### Configuration on SW4

1. **Create and Assign VLAN 60**:

   ```plaintext
   SW4> enable
   SW4# configure terminal
   SW4(config)# vlan 60
   SW4(config-vlan)# name VLAN60
   SW4(config-vlan)# exit

   SW4(config)# interface range fa0/1 - 24
   SW4(config-if-range)# switchport mode access
   SW4(config-if-range)# switchport access vlan 60
   SW4(config-if-range)# exit
   ```

2. **Configure Trunk Port to Router R2**:
   ```plaintext
   SW4(config)# interface fa0/24
   SW4(config-if)# switchport mode trunk
   SW4(config-if)# switchport trunk encapsulation dot1q
   SW4(config-if)# exit
   ```

#### Configuration on Dedicated DHCP Server (for SW4)

Assuming the dedicated DHCP server is a Cisco device or another server capable of running a DHCP service:

1. **Enter Global Configuration Mode**:

   ```plaintext
   DHCP-SERVER> enable
   DHCP-SERVER# configure terminal
   ```

2. **Configure DHCP Pool for SW4 Devices**:
   ```plaintext
   DHCP-SERVER(config)# ip dhcp pool SW4_POOL
   DHCP-SERVER(dhcp-config)# network 192.168.60.0 255.255.255.0
   DHCP-SERVER(dhcp-config)# default-router 192.168.60.1
   DHCP-SERVER(dhcp-config)# dns-server 8.8.8.8 8.8.4.4
   DHCP-SERVER(dhcp-config)# exit
   ```

#### Verification Commands

1. **Verify DHCP Pools on Router R2**:

   ```plaintext
   R2# show ip dhcp pool
   ```

2. **Verify DHCP Bindings on Router R2**:

   ```plaintext
   R2# show ip dhcp binding
   ```

3. **Verify VLANs on Switch SW3**:

   ```plaintext
   SW3# show vlan brief
   ```

4. **Verify Trunk Ports on Switch SW3**:

   ```plaintext
   SW3# show interfaces trunk
   ```

5. **Verify VLANs on Switch SW4**:

   ```plaintext
   SW4# show vlan brief
   ```

6. **Verify Trunk Ports on Switch SW4**:
   ```plaintext
   SW4# show interfaces trunk
   ```

By following these steps, you will have configured your network to ensure that devices under SW3 obtain IP addresses from router R2 via DHCP, while devices under SW4 obtain IP addresses from a dedicated DHCP server.

### (b)

1. **Hostnames**:

   - **R1**: `hostname R1`
   - **SW1**: `hostname SW1`
   - **SW3**: `hostname SW3`

2. **Telnet Configuration**:

   - **R1**:
     ```plaintext
     line vty 0 4
     login local
     transport input telnet
     ```
   - **SW1**:
     ```plaintext
     line vty 0 4
     login local
     transport input telnet
     ```
   - **SW3**:
     ```plaintext
     line vty 0 4
     login local
     transport input telnet
     ```

3. **SSH Configuration**:
   - **R1**:
     ```plaintext
     username ITER privilege 15 secret ITER
     ip domain-name example.com
     crypto key generate rsa
     line vty 0 4
     transport input telnet ssh
     ip ssh version 2
     ip ssh authentication-retries 3
     ```
   - **SW3**:
     ```plaintext
     username ITER privilege 15 secret ITER
     ip domain-name example.com
     crypto key generate rsa
     line vty 0 4
     transport input telnet ssh
     ip ssh version 2
     ip ssh authentication-retries 3
     ```

These commands configure hostnames, enable Telnet and SSH access on R1 and SW3, and set up local user authentication with the username and password both set to "ITER".

## Question (6)

### (a)

1. **Create ACL 101**:

   ```plaintext
   R1(config)# access-list 101 deny tcp host 192.168.1.9 any eq 23
   R1(config)# access-list 101 deny tcp 192.168.1.10 0.0.0.6 any eq 23
   R1(config)# access-list 101 permit ip any any
   ```

2. **Apply ACL 101 to VTY lines**:
   ```plaintext
   R1(config)# line vty 0 4
   R1(config-line)# access-class 101 in
   R1(config-line)# exit
   ```

### Explanation

- `access-list 101 deny tcp host 192.168.1.9 any eq 23`: This command denies TCP traffic from PC9 (IP `192.168.1.9`) to any destination on port 23 (Telnet).
- `access-list 101 deny tcp 192.168.1.10 0.0.0.6 any eq 23`: This command denies TCP traffic from the IP range `192.168.1.10` to `192.168.1.15` (using a wildcard mask of `0.0.0.6`) to any destination on port 23 (Telnet).
- `access-list 101 permit ip any any`: This command permits all other traffic.
- `access-class 101 in`: This command applies the ACL to incoming connections on the VTY lines, effectively controlling Telnet access.

With these commands, you have configured the router to deny Telnet access to PC9 and the next 6 hosts in the same subnet while allowing all other traffic.

### (b)

1. **Verifying Access List**:

   ```plaintext
   R1# show access-lists 101
   R1# show access-lists
   R1# show running-config | include access-class
   ```

2. **Creating and Applying Named Standard ACL**:
   ```plaintext
   R1> enable
   R1# configure terminal
   R1(config)# ip access-list standard BLOCK_PC9_AND_OTHERS
   R1(config-std-nacl)# deny 192.168.1.9
   R1(config-std-nacl)# deny 192.168.1.10 0.0.0.6
   R1(config-std-nacl)# permit any
   R1(config-std-nacl)# exit
   R1(config)# line vty 0 4
   R1(config-line)# access-class BLOCK_PC9_AND_OTHERS in
   R1(config-line)# exit
   ```
