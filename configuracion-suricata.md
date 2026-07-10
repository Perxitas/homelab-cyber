    # 01 — Setup Suricata en el firewall con reglas ETopen

    ## Objetivo
    Activar el IDS de suricata y detectar alertas segun un set de reglas de ETopen, afinar reglas si es que se requiere para falsos positivos detectados por la IDS, para luego aplicar el IPS, bloqueando tráfico que sí lo requiere.

    ## ¿Que es Suricata?
    Es un sistema IDS/IPS, esto significa que detecta y previene intrusos. Este es un paquete adicional para complementar con una inspección más a fondo de paquetes que lo que el firewall trae predeterminado.

    ## Instalación
    Dentro de pfSense 
            
        Sistema
            |
        Package manager
            |
        Available Packages
            |
        Suricata 7.08_5

## Configuración
Primero destacar que debemos remover de System > Advanced > Networking. Las opciones de Hardware TCP Segmentation Offloading y Hardware Large Recieve Offloading (Activados de forma predeterminada). Esto debido a que modifican el paquete previamente y por lo tanto Suricata no lee el paquete completo sino una versión alterada.

En Services > Suricata > Global Settings Instalamos las reglas ETOpen, especificamente ETOPen Emerging Threats rules. Estas reglas son firmas de código que monitorean tráfico, comportamientos malicios y pueden bloquear ataques por lo tanto que son actualizadas frecuentemnete y vienen de forma facíl para su integración en suricata

Luego, en Services > Suricata > Interfaces, añadimos una nueva interfaz, la interfaz a la que Suricata se aplica que es LAN(em1), creamos y en mi caso tuve un problema, no me aparecían las reglas del ETOpen, para esto tuve que actualizarlas, esto fue en Suricata > Updates > Click en el boton de update bajo UPDATE YOUR RULE SET. Luego selecciones las reglas de las que iba a estar definido inicialmente las alertas (Esta interfaz es solo IDS), en LAN Categories emerging-exploit.rules, emerging-malware.rules, emerging-scan.rules en un principio. Estas reglas para empezar y no sobrecargar la VM.

## Prueba
De manera inicial, un comando básico, nmap -sS -A 192.168.1.1 para obtener alertas en nuestro log de Alerts en Suricata. 

![Alertas Suricata](imgs/suricata-alertas1.png)

-Se pueden observar que las primeras fueron de los paquetes ICMP para el descubrimiento de hosts, que suricata no reconoce.

![Alertas Suricata](imgs/suricata-alertas0.png)

-También estuvo entremedio de las alertas una de tipo Applayer detect protocol only one direction, esto se debe a que nmap con el -sS, no completa el handshake. Por lo tanto Suricata ve tráfico en 1 sola direccion, no llegando a capturar el protocolo.

## Próximo paso
- [ ] Evaluar si emerging-scan detecta SYN scans puros o requiere ajuste de reglas
- [ ] Se debe activar Block Offenders para que nuestra IDS pase a modo IPS y podamos realmente bloquear tráfico malicioso

## Observaciones
- nmap -sS solo no generó alertas, requirió -A para ser alertado.
- La alerta fue por el user agent HTTP de nmap, no por el SYN scan