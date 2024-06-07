# pyq some questions from 2023 set 1

## Question (4) (a)

Write the CLI command for given task [5]
(i) Which command would you run to bypass the configuration in NVRAM?
(ii) What is the command to show MAC address table in Cisco
router?
(iii) Which command is used to copy configuration file from
RAM to NVRAM?
(iv) Write Command to delete configuration stored in
NVRAM?

1. To bypass the configuration in NVRAM (non-volatile RAM), you can use the following command:

   ```
   write erase
   ```

   This command will erase the startup configuration stored in NVRAM, effectively resetting the router to its default state².

   OR

   ```
   confreg 0x2142 (in ROMMON mode)
   config-register 0x2142 (in configuration mode)
   ```

2. To show the MAC address table in a Cisco router, use the following command:

   ```
   show mac address-table
   ```

   This command lists all the MAC addresses learned by the switch (if the router has a switching module installed) and associates them with VLANs and interfaces⁵.

3. To copy the configuration file from RAM (volatile memory) to NVRAM, use:

   ```
   copy running-config startup-config
   ```

   This command saves the current running configuration to NVRAM, making it persistent across reboots².

4. To delete the configuration stored in NVRAM, you can issue:
   ```
   erase startup-config
   ```
   This command removes the startup configuration from NVRAM, effectively wiping out any saved settings².

(b)
Based on the provided diagram and requirements, here are the commands to configure static NAT on the ITER router to translate the private IP addresses of the 12.0.0.0 network to the public IP address range 50.50.50.2 to 50.50.50.20.

### Static NAT Configuration on ITER Router

#### (i) Static NAT Configuration Commands

```plaintext
Router(config)# ip nat inside source static 12.0.0.1 50.50.50.2
Router(config)# ip nat inside source static 12.0.0.2 50.50.50.3
Router(config)# ip nat inside source static 12.0.0.3 50.50.50.4
```

These commands create static NAT mappings for specific devices in the 12.0.0.0 network.

#### (ii) Implement Static NAT Inside for the Interface

You need to specify which interface on the ITER router is connected to the internal (private) network.

```plaintext
Router(config)# interface Ethernet0
Router(config-if)# ip nat inside
Router(config-if)# exit
```

This sets the Ethernet0 interface as the inside interface for NAT.

#### (iii) Implement Static NAT Outside for the Interface

You need to specify which interface on the ITER router is connected to the external (public) network (Internet).

```plaintext
Router(config)# interface Ethernet1
Router(config-if)# ip nat outside
Router(config-if)# exit
```

This sets the Ethernet1 interface as the outside interface for NAT.

#### (iv) Verify Static NAT Translation

To verify that the static NAT translations are correctly configured and active, you can use the following command:

```plaintext
Router# show ip nat translations
```

## Question (5)

### Part (a): Configuring DHCP on Router R1 for Network 14.0.0.0/8

To create a DHCP server on Router R1 for the 14.0.0.0/8 network, follow these steps:

1. **Create a DHCP Pool**:

   ```
   R1(config)# ip dhcp pool VLAN_14
   ```

2. **Specify the Following**:

   - **Network**: Define the network and subnet mask for the DHCP pool (14.0.0.0/8):
     ```
     R1(dhcp-config)# network 14.0.0.0 255.0.0.0
     ```
   - **Default Router (Default Gateway)**: Set the default gateway for DHCP clients (e.g., R1's interface IP):
     ```
     R1(dhcp-config)# default-router 14.0.0.1
     ```
   - **DNS Servers**: Specify DNS server(s) for DHCP clients (replace with actual DNS server IPs):
     ```
     R1(dhcp-config)# dns-server 14.0.0.10 14.0.0.20
     ```

3. **Exclude Addresses**:
   Exclude any addresses reserved for static assignment (e.g., router interface IP):

   ```
   R1(config)# ip dhcp excluded-address 14.0.0.1 14.0.0.20
   ```

4. **Verify DHCP Pools**:
   ```
   R1# show ip dhcp pool
   ```

### Part (b): Setting Up VTY Lines for Telnet Access

To configure VTY lines for telnet access on devices SW-1, R1, SW-2, and R2, use the following commands:

1. **Enter Line Configuration Mode**:

   ```
   SW-1(config)# line vty 0 4
   ```

2. **Set Authentication**:

   ```
   SW-1(config-line)# login
   ```

3. **Set Username and Password** (use your desired username and password):

   ```
   SW-1(config-line)# username iter-cnw password iter-cnw
   ```

4. **Specify Allowed Transport Input** (telnet in this case):
   ```
   SW-1(config-line)# transport input telnet
   ```

Repeat the same steps for R1, SW-2, and R2.

## Question (6)

(a) Refer Figure 4. Utilizing network, A (R1 as router, SW1 as
switch and CP-1, CP-2, CP-3 as PC) to implement router on a
stick (ROAS) Inter-VLAN routing. Create necessary VLANs
and do write down the necessary configurations.
(b) Refer to Figure 4. Enable RIP routing within R1, and R2 so
that all devices can communicate with each other. Write down
the necessary configurations w.r.t each route

### Part (a): Configuring Router on a Stick (ROAS) for Inter-VLAN Routing

To implement router-on-a-stick (ROAS) Inter-VLAN routing on Network A with R1 as the router, SW1 as the switch, and CP-1 to CP-3 as PCs, follow these steps:

1. **Create VLANs on SW1**:

   ```
   SW1(config)# vlan 10
   SW1(config-vlan)# name VLAN10
   SW1(config-vlan)# vlan 20
   SW1(config-vlan)# name VLAN20
   SW1(config-vlan)# vlan 30
   SW1(config-vlan)# name VLAN30
   ```

2. **Assign VLANs to Switch Ports Connected to CPs**:

   ```
   SW1(config)# interface FastEthernet0/1
   SW1(config-if)# switchport mode access
   SW1(config-if)# switchport access vlan 10
   SW1(config-if)# interface FastEthernet0/2
   SW1(config-if)# switchport mode access
   SW1(config-if)# switchport access vlan 20
   SW1(config-if)# interface FastEthernet0/3
   SW1(config-if)# switchport mode access
   SW1(config-if)# switchport access vlan 30
   ```

3. **Configure Subinterfaces on R1 for Each VLAN**:

   ```
   R1(config)# interface FastEthernet0/0.10
   R1(config-subif)# encapsulation dot1Q 10
   R1(config-subif)# ip address 12.0.0.1 255.255.255.0

   R1(config-subif)# interface FastEthernet0/0.20
   R1(config-subif)# encapsulation dot1Q 20
   R1(config-subif)# ip address 12.0.1.1 255.255.255.0

   R1(config-subif)# interface FastEthernet0/0.30
   R1(config-subif)# encapsulation dot1Q 30
   R1(config-subif)# ip address 12.0.2.1 255.255.255.0
   ```

4. **Enable Routing on R1**:
   ```
   R1(config)# ip routing
   ```

### Part (b): Enabling RIP Routing on R1 and R2

To enable RIP routing within routers R1 and R2 so that all devices can communicate with each other, follow these steps:

1. **Configure RIP on R1**:

   ```
   R1(config)# router rip
   R1(config-router)# version 2
   R1(config-router)# network 12.0.0.0
   R1(config-router)# no auto-summary
   R1(config-router)# exit
   ```

2. **Configure RIP on R2**:
   ```
   R2(config)# router rip
   R2(config-router)# version 2
   R2(config-router)# network 14.0.0.0
   R2(config-router)# no auto-summary
   R2(config-router)# exit
   ```
