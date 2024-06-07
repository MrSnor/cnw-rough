# pyq some questions from 2023 set 2

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

Certainly! Let's address each part of your question:

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

