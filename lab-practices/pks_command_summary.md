# PKS -- Sumár príkazov / Command Summary

## Úvodná konfigurácia zariadenia / Initial Device Configuration

    Router> enable
    Router# configure terminal

### Nastavenie hostname / Setting the hostname

    Router(config)# hostname <name>

### Vypnutie prekladu domén / Disabling domain lookup

    Router(config)# no ip domain-lookup

### Nastavenie hlášok / Setting system messages (banners)

    Router(config)# banner motd # <message> #
    Router(config)# banner login # <message> #

### Synchrónne logovanie / Enabling synchronous logging

    Router(config)# line console 0
    Router(config-line)# logging synchronous

### Uloženie konfigurácie / Saving the configuration

    Router# write
    Router# copy running-config startup-config

------------------------------------------------------------------------

## Zabezpečenie zariadenia / Device Security

### Skrývanie plain-text hesiel / Hiding plain-text passwords

    Router(config)# service password-encryption

### Zabezpečenie privilegovaného režimu / Securing privileged mode

    Router(config)# enable secret <password>

### Zabezpečenie konzoly / Securing console access

    Router(config)# line console 0
    Router(config-line)# password <password>
    Router(config-line)# login

------------------------------------------------------------------------

## Konfigurácia interfacov / Interface Configuration

### Konfigurácia jedného interface / Configuration of an interface

    Router(config)# interface <interface> <interface-num>

### Konfigurácia viacerých interface naraz / Configuration of multiple interfaces at once

    Router(config)# interface range <interface> <interface-start>-<interface-end>

Example:

    Router(config)# interface range fa0/1-24

### Nastavenie IPv4 adresy / IPv4 address configuration

    Router(config-if)# ip address <ip-address> <mask>

### Nastavenie IPv6 GUA adresy / IPv6 GUA address configuration

    Router(config-if)# ipv6 address <ipv6-GUA-address>/<prefix-length>

### Nastavenie IPv6 link-local adresy / IPv6 link-local address configuration

    Router(config-if)# ipv6 address <ipv6-LL-address> link-local

### Nastavenie description / Interface description configuration

    Router(config-if)# description <description>

### Aktivácia portu / Bring interface up

    Router(config-if)# no shutdown

------------------------------------------------------------------------

## Pripojenie klienta cez SSH / SSH Client

    Router# ssh -l <username> <ip-addr>

------------------------------------------------------------------------

## Nastavenie statickej cesty / Configure static routes

### IPv4

    Router(config)# ip route <dest-ip> <dest-mask> <next-hop-ip>

### IPv6

    Router(config)# ipv6 route <dest-prefix>/<prefix-length> <next-hop-ip>

------------------------------------------------------------------------

## Povolenie IPv6 smerovania / Enable IPv6 Routing

    Router(config)# ipv6 unicast-routing

------------------------------------------------------------------------

## Predvolená cesta na prepínači / Default Gateway on Switch

    Switch(config)# ip default-gateway <default-ip>

------------------------------------------------------------------------

## Verifikačné & diagnostické príkazy / Verification & diagnostic commands

    Router/Switch# ping <ip-addr>
    Router/Switch# traceroute <ip-addr>
    Router/Switch# show ip interface [brief]
    Router/Switch# show ipv6 interface [brief]
    Router/Switch# show interfaces
    Router# show ip route
    Router# show ipv6 route
    Router/Switch# show running-config
    Router/Switch# show startup-config
    Switch# show interfaces status
