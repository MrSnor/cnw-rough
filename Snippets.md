# Cisco snippets

- [ ] Subnetting, VLSM 
- [x] VLAN and trunking
- [ ] STP, DTP, VTP ether channel
- [ ] DHCP
- [ ] Routing
- [ ] ACL
- [ ] Network connectivity and services
- [x] SSH

## Vlan configuration

`switchport mode access` - is used to  
`switchport mode trunk` - is used for  
`sh flash` - 

### single vlan for each switch

Switch 1 (1st vlan)
```
Switch(config)#vlan 100
Switch(config-vlan)#name test1
Switch(config-vlan)#int range fa0/1-2
Switch(config-if-range)#switchport access vlan 100
Switch(config-if-range)#do sh vlan brief

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Fa0/3, Fa0/4, Fa0/5, Fa0/6
                                                ...
100  test1                            active    Fa0/1, Fa0/2
1002 fddi-default                     active    
1003 token-ring-default               active    
1004 fddinet-default                  active 
```

Switch 2 (2nd vlan)
```
Switch(config)#vlan 200
Switch(config-vlan)#name test2
Switch(config-vlan)#int range fa0/3-4
Switch(config-if-range)#switchport access vlan 200
Switch(config-if-range)#do sh vlan brief

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Fa0/1, Fa0/2, Fa0/5, Fa0/6
                                                ...
200  test2                            active    Fa0/3, Fa0/4
1002 fddi-default                     active    
1003 token-ring-default               active    
1004 fddinet-default                  active    
1005 trnet-default                    active    
Switch(config-if-range)#
```

### Configuring trunk for each switch's connecting port (using a single wire to connect the two switches directly)

Switch 1
```
Switch(config)#int fa0/5
Switch(config-if)#switchport mode trunk
Switch(config-if)#switchport trunk allowed vlan 100,200
Switch(config-if)#do sh int fa0/5 switchport
Name: Fa0/5
Switchport: Enabled
Administrative Mode: trunk
Operational Mode: trunk
...
Trunking VLANs Enabled: 100,200
```

Switch 2 (optional, since trunking on the other switch  
automatically enables it here)
```
Switch(config)#int fa0/5
Switch(config-if)#switchport mode trunk
Switch(config-if)#switchport trunk allowed vlan 100,200
Switch(config-if)#do sh int fa0/5 switchport
Name: Fa0/5
Switchport: Enabled
Administrative Mode: trunk
Operational Mode: trunk
...
Trunking VLANs Enabled: 100,200
```

**Trunk Link Established**: Both Switch 1 and Switch 2 have their FastEthernet 0/5 interfaces configured as trunk ports. This setup allows the two switches to exchange traffic for multiple VLANs over a single physical link.

**VLAN Traffic Restricted**: The trunk link between Switch 1 and Switch 2 will only carry traffic for VLANs 100 and 200. Any other VLAN traffic will be blocked from traversing this trunk link.

### Inter-vlan communication (Using a Router (Router-on-a-Stick))

`sh int trunk` - displays information about the trunk ports on the switch. It shows details such as the trunking mode, encapsulation type (e.g., 802.1Q), allowed VLANs, and active VLANs on each trunk port.


```
Router(config)# interface fastEthernet 0/0.100
Router(config-subif)# encapsulation dot1Q 100
Router(config-subif)# ip address 192.168.1.1 255.255.255.0
Router(config-subif)# exit
Router(config)# interface fastEthernet 0/0.200
Router(config-subif)# encapsulation dot1Q 200
Router(config-subif)# ip address 192.168.2.1 255.255.255.0
Router(config-subif)# exit
```


## Ssh config

Configure ssh for other devices to connect to your device

```
Switch (config) #hostname S1
S1 (config) #enable password 1234
S1 (config) #ip domain-name blnp.local

S1 (config) #crypto key generate rsa
The name for the keys will be: S1.blnp.local
Choose the size of the key modulus in the range of 360 to 4096 for your
...
How many bits in the modulus [512]: 2048
% Generating 2048 bit RSA keys, keys will be non-exportable ... [OK]

S1 (config) #
*Mar 1 1:28:3.900: %SSH-5-ENABLED: SSH 1.99 has been enabled
S1 (config) #
S1 (config) #ip ssh version 2
S1 (config) #line vty 0 12
S1 (config-line) #transport input ssh
S1(config-line)#login local
S1(config-line)#exit 
S1(config)#username soa password 1234
```

Connecting from the client side

```
C:\>ssh -l soa 10.10.10.2

Password: 

S1>en
Password: 
S1#exit
```