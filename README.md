#  Cisco Device Hardening & Syslog Logging

A quick-reference guide for securing Cisco routers and switches with login attempt control, remote logging, and OSPF authentication.

---

##  Basic Device Security
```
! Set hostname
hostname SECURE-ROUTER

! Set strong enable secret
enable secret STRONG_PASSWORD

! Create local admin user
username admin privilege 15 secret STRONG_ADMIN_PASS

! Disable unnecessary services
no ip http server
no ip http secure-server
no cdp run
no ip domain-lookup

```

 
## Secure VTY Access (SSH Only)
```
! Configure domain and SSH
ip domain-name yourdomain.local
crypto key generate rsa
ip ssh version 2

! Restrict access to trusted subnet
access-list 10 permit 192.168.1.0 0.0.0.255

! Secure VTY lines
line vty 0 4
 login local
 transport input ssh
 access-class 10 in
 exec-timeout 5 0
 logging synchronous
```


## Login Attempt Blocking & Logging
```
! Block brute-force login attempts
login block-for 15 attempts 3 within 60
login quiet-mode access-class PERMIT-ADMIN
! Log failed and successful logins
login delay 10
login on-failure log
login on-success log
```
## Disable Unused Ports

```
! Create unused VLAN
vlan 999
 name UNUSED

! Assign unused ports to it and shut them down
interface range fa0/10 - 24
 switchport mode access
 switchport access vlan 999
 shutdown
```

## OSPF Authentication 
```
interface <INTERFACE>
 ip ospf authentication message-digest
 ip ospf message-digest-key 1 md5 STRONG_KEY
```
