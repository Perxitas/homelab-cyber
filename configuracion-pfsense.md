# 01 — Setup pfSense como firewall perimetral

## Objetivo

Levantar pfSense como firewall entre internet y una red interna virtual, replicando una arquitectura básica de un firewall perimetral.

## Arquitectura

```
Internet (WAN)
     │
  em0 Adaptador puente > red real del host
     │
  pfSense 
  em1 Red interna VirtualBox 
     │
  192.168.1.0/24
     │
  Kali Linux 
```

## Configuración de la VM (VirtualBox)

| Parámetro | Configuración |
|---|---|
| OS | FreeBSD |
| RAM | 512 MB |
| Disco | 8 GB (ZFS) |
| Adaptador 1 (WAN) | Adaptador puente Realtek PCIe GbE |
| Adaptador 2 (LAN) | Red interna `lan_pfsense` |

## Instalación

1. ISO descargada desde pfsense.org: AMD64 ISO IPMI/Virtual Machines
2. Instalación con ZFS + GPT sobre disco virtual
3. WAN asignada a `em0` (DHCP), LAN a `em1` (192.168.1.1/24)

## Configuración inicial (Web GUI)

Acceso desde Kali: `http://192.168.1.1`.

| Parámetro | Valor/Nombres |
|---|---|
| Hostname | pfsense-homelab |
| Domain | homelab.lan |
| DNS primario | 8.8.8.8 |
| DNS secundario | 1.1.1.1 |
| Timezone | America/Santiago |
| LAN IP | 192.168.1.1/24 |

## Por qué?

pfSense actúa como un firewall perimetral, separando la red WAN (internet) de la red LAN (interna). Todo el tráfico viaja por pfSense, el cual aplicará reglas del firewall.  

## Próximo paso

[1/3] Configurar reglas de firewall, segmentación con VLANs y añadir Suricata como IDS/IPS.

## Corrección de arquitectura de red

### Problema
WAN y LAN estaban en la misma subred (192.168.1.x), causando que pfSense 
no pudiera entender de forma correcta como funciona el tráfico y enviarlo correctamente a internet.

Algo que se detectó al momento de configurar reglas de firewall y probar conectividades. 

### Solución
- Cambiar LAN de 192.168.1.1/24 a 10.0.0.1/24

### Arquitectura nueva
Internet
    │
Router (192.168.100.x)
    │
pfSense WAN (192.168.100.205)
    │
pfSense LAN (10.0.0.1)
    │
Kali (10.0.0.102)