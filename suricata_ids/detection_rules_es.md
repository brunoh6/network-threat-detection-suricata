# Laboratorio IDS Suricata – Entorno EVE-NG

## Objetivo

Implementar y configurar Suricata como un sistema de detección de intrusos en red (NIDS) dentro de un entorno empresarial simulado en EVE-NG. El laboratorio busca detectar ataques comunes como fuerza bruta SSH, inundaciones ICMP y escaneo de puertos, utilizando reglas personalizadas y firmas por defecto.

## Entorno del Laboratorio

- **Plataforma:** EVE-NG Community Edition
- **Nodos:**
  - GW1: Router Cisco
  - SW1: Switch vIOS (configurado con SPAN)
  - Kali Linux: Nodo atacante
  - Ubuntu Server: Nodo con Suricata
  - VPC: Nodo objetivo (IP por DHCP)
- **Red:** 192.168.6.0/24
- **Acceso a Internet:** Vía GW1 (NAT + DHCP)

## Configuraciones Clave

- **Router GW1**
  - NAT overload con access-list
  - DHCP pool con exclusiones
  - Ruta por defecto: `0.0.0.0/0 -> 192.168.36.2`

- **Puerto SPAN en SW1**
  - Tráfico de Gi0/2 replicado a Gi1/0 (Suricata)

## Configuración de Suricata

1. **Interfaz**
   - `ens4` (e1) con DHCP: 192.168.6.13
   - Modo promiscuo activado con servicio systemd

2. **Instalación**
   - Suricata v6.0.4 instalada vía PPA
   - `suricata.yaml` modificado:
     - `HOME_NET: [192.168.6.0/24]`
     - `af-packet` monitoreando `ens4`

3. **Reglas**
   - Regla personalizada para tráfico HTTP: `sid:1000002`
   - Detección de ataques SYN-flood con `hping3`
   - Regla de detección de Kali Linux deshabilitada: `sid:2022973`

4. **Generación de Tráfico**
   - Pings ICMP desde VPC
   - Acceso HTTP al servidor Apache2
   - Escaneo Nmap y SYN-flood desde Kali

5. **Análisis de Logs**
   - Revisión de `/var/log/suricata/fast.log`
   - Captura de paquetes con `tcpdump`
   - Correlación con Wireshark

## Resultados

- Se detectaron correctamente ICMP, SYN-flood y patrones HTTP
- Escaneos pasivos con Nmap no fueron detectados por defecto
- Desactivación exitosa de regla ruidosa para Kali

## Conclusiones

Suricata combinado con EVE-NG y puertos SPAN permite un entorno seguro para probar y ajustar reglas IDS. Se pueden evaluar patrones de comportamiento sin afectar sistemas reales.
