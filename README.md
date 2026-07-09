# homelab-cyber

Laboratorio de ciberseguridad con un enfoque a arquitectura de redes seguras y seguridad en la nube.

## Objetivo del proyecto

Construir y documentar una arquitectura de seguridad funcional desde cero, aplicando conceptos de firewalls, segmentación de red, Zero Trust y cloud security.

## Arquitectura actual

```
Internet
   │
   ▼
[pfSense Firewall] VM en VirtualBox 
   │
   ▼
[Red interna: 192.168.1.0/24]
   │
   ▼
[Kali Linux] VM cliente para administración y pentesting
```

## Stack

| Componente |
|---|---|
| Hypervisor | VirtualBox |
| Firewall | pfSense 2.8.1 CE |
| Client / Pentesting | Kali Linux 2026.2 |

## Proyectos

| # | Proyecto | Estado |
|---|---|---|
| 01 | [Configuración pfSense + red interna](configuracion-pfsense.md) | Completado |

## Objetivos a cumplir/Roadmap

- [ ] Segmentación de red con VLANs
- [ ] Reglas de firewall y hardening
- [ ] IDS/IPS con Suricata
- [ ] VM vulnerable para práctica de pentesting
- [ ] Integración con cloud (AWS VPC)
- [ ] Arquitectura Zero Trust
