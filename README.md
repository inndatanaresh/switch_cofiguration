# Switch Configuration

## Agenda
1. Assigning hostname to the switch
2. Creating VLANs
3. Assigning VLANs to ports
4. Configuring VLAN interfaces for management
5. Setting up the default gateway
6. Verifying and saving the configuration
7. Setting up trunking between Switch 1 and Switch 2
8. Assigning different IPs to VLANs for network communication
9. Enabling IP routing on all ports

---

## Enter Global Configuration Mode
```bash
# enable (shortcut: en)
# configure terminal
```

## Set the Hostname
```bash
# hostname switch1
```

## Setup VLANs
```bash
# vlan 5
# name proxmox_cluster_and_vms
# exit

# vlan 6
# name proxmox_cororsync_network
# exit

# vlan 10
# name idrac
# exit

# vlan 80
# name proxmox_cluster
# exit
```

---

## Assign VLANs to Ports
```bash
# interface range fa0/1 – 12
# switchport mode access
# switchport access vlan 5
# exit

# interface range fa0/13 – 16
# switchport mode access
# switchport access vlan 6
# exit

# interface range fa0/17 - 20
# switchport mode access
# switchport access vlan 10
# exit

# interface range fa0/21 – 24
# switchport mode access
# switchport access vlan 80
# exit
```

---

## Assign IPs to VLANs (Switch 1)
```bash
# interface vlan 5
# ip address 10.10.5.2 255.255.255.0
# no shutdown
# exit

# interface vlan 6
# ip address 10.10.6.1 255.255.255.0
# no shutdown
# exit

# interface vlan 10
# ip address 192.168.0.1 255.255.255.0
# no shutdown
# exit

# interface vlan 80
# ip address 10.10.80.11 255.255.255.0
# no shutdown
# exit
```

---

## Verify and Save Configuration
```bash
# show vlan brief
# show ip interface brief
# end
# write memory
# copy running-config startup-config
```

---

## Set Default Gateway (Switch 1)
```bash
# ip default-gateway 10.10.5.222
```

---

## Assign IPs to VLANs (Switch 2)
```bash
# interface vlan 5
# ip address 10.10.5.3 255.255.255.0
# no shutdown
# exit

# interface vlan 6
# ip address 10.10.6.1 255.255.255.0
# no shutdown
# exit

# interface vlan 10
# ip address 192.168.0.1 255.255.255.0
# no shutdown
# exit

# interface vlan 80
# ip address 10.10.80.11 255.255.255.0
# no shutdown
# exit
```

---

## Set Default Gateway (Switch 2)
```bash
# ip default-gateway 10.10.5.222
```

---

## High Availability Between Switches (Trunk Port Configuration)
```bash
# interface fa0/24
# switchport mode trunk
# switchport trunk allowed vlan 5,6,10,80
```

---

## Enable IP Routing on Switch
```bash
# interface range fa0/1 – 24
# switchport mode trunk
# switchport trunk allowed vlan all
# exit
```
(Alternative command)
```bash
# interface range fa0/1 – 24
# switchport mode trunk
# switchport trunk allowed vlan 5,6,10,80
# exit
# write memory
# copy running-config startup-config
```

---

## The above process is the same for Switch 2

## reference link

# https://chatgpt.com/share/d234f895-ec12-4b19-9d1d-99f14abe68f9
