# 🌐 Proyecto de Redes: Infraestructura Empresarial con Cisco CCNA

Este repositorio contiene la documentación y configuración de una topología de red empresarial simulada. El proyecto integra protocolos de enrutamiento dinámico, alta disponibilidad, seguridad de acceso y segmentación mediante VLANs, cumpliendo con los estándares de diseño jerárquico.

##  Equipo de Trabajo

| Nombre | Matrícula | Rol |
| :--- | :--- | :--- |
| **Isamar Vásquez Santana** | 2025-0196 | Líder del equipo |
| **Lisbeth Gómez Daian Gómez** | 2025-0701 | Integrante |
| **Keury Eliezer Rijo Pérez** | 2025-2137 | Integrante |
| **Estarlyn Vallejo** | 2025-2179 | Integrante |
| **José Cabral** | 2025-1631 | Integrante |

---

##  Credenciales de Acceso Base

Todos los dispositivos de la infraestructura comparten los siguientes parámetros de seguridad y acceso:

| Parámetro | Valor Configurado |
| :--- | :--- |
| **Usuario** | `admin` |
| **Contraseña de usuario** | `itla123` |
| **Enable secret** | `Cisco123` |
| **Exec-timeout** | 10 minutos |
| **Acceso remoto** | Telnet |
| **Acceso local** | Consola |

> **Nota de Seguridad:** Todas las contraseñas están encriptadas utilizando el comando `service password-encryption`. El acceso a cualquier equipo exige autenticación local estricta.

---

##  Mapa de Puertos y Topología

###  Routers

| Dispositivo | Puerto | Conectado a | VLANs / Función | Modo |
| :--- | :--- | :--- | :--- | :--- |
| **ISP** | `e0/0` | R1-Core `e0/2` | WAN 203.0.113.1/30 | Routed |
| **R1-Core** | `e0/0` | SWM1 `e0/0` | 10, 20, 30, 40, 99 | Trunk |
| **R1-Core** | `e0/1` | SWM2 `e0/0` | 10, 20, 30, 40, 99 | Trunk |
| **R1-Core** | `e0/2` | ISP `e0/0` | WAN 203.0.113.2/30 | Routed |
| **R1-Core** | `s1/0` | R2 `s1/0` | P2P 172.18.10.161/30 | Serial |
| **R2** | `e0/0` | SWA13 `e0/0` | 50, 60, 99 | Trunk |
| **R2** | `e0/1` | SW `e0/0` | 70, 99 | Trunk |
| **R2** | `s1/0` | R1-Core `s1/0`| P2P 172.18.10.162/30 | Serial |

###  Switches de Distribución

| Dispositivo | Puerto | Conectado a | VLANs / Función | Modo |
| :--- | :--- | :--- | :--- | :--- |
| **SWM1** | `e0/0` | R1-Core `e0/0` | 10, 20, 30, 40, 99 | Trunk |
| **SWM1** | `e0/1-3`| SWM2 `e0/1-3` | EtherChannel Po1 | Trunk |
| **SWM1** | `e1/0` | SWAC1 `e0/0` | 20, 99 | Trunk |
| **SWM1** | `e1/1` | SWAC2 `e0/0` | 10, 99 | Trunk |
| **SWM1** | `e1/2` | SWAC3 `e0/0` | 30, 40, 99 | Trunk |
| **SWM2** | `e0/0` | R1-Core `e0/1` | 10, 20, 30, 40, 99 | Trunk |
| **SWM2** | `e0/1-3`| SWM1 `e0/1-3` | EtherChannel Po1 | Trunk |
| **SWM2** | `e1/0` | SWAC1 `e0/1` | 20, 99 | Trunk |
| **SWM2** | `e1/1` | SWAC2 `e0/1` | 10, 99 | Trunk |
| **SWM2** | `e1/2` | SWAC3 `e0/1` | 30, 40, 99 | Trunk |

### 🔌 Switches de Acceso

| Dispositivo | Puerto | Conectado a | VLANs / Función | Modo |
| :--- | :--- | :--- | :--- | :--- |
| **SWAC1** | `e0/0` | SWM1 `e1/0` | 20, 99 | Trunk |
| **SWAC1** | `e0/1` | SWM2 `e1/0` | 20, 99 | Trunk |
| **SWAC1** | `e1/0` | DOC1 | 20 | Access |
| **SWAC2** | `e0/0` | SWM1 `e1/1` | 10, 99 | Trunk |
| **SWAC2** | `e0/1` | SWM2 `e1/1` | 10, 99 | Trunk |
| **SWAC2** | `e2/0` | EST1 | 10 | Access |
| **SWAC3** | `e0/0` | SWM1 `e1/2` | 30, 40, 99 | Trunk |
| **SWAC3** | `e0/1` | SWM2 `e1/2` | 30, 40, 99 | Trunk |
| **SWAC3** | `e3/0` | GES1 | 30 | Access |
| **SWA13** | `e0/0` | R2 `e0/0` | 50, 60, 99 | Trunk |
| **SWA13** | `e1/0` | ACA1 | 60 | Access |
| **SWA13** | `e2/0` | FIN1 | 50 | Access |
| **SW** | `e0/0` | R2 `e0/1` | 70, 99 | Trunk |
| **SW** | `e0/1` | SERVICIOS | 70 | Access |

---

##  Tecnologías Implementadas

### 1. VTP (VLAN Trunking Protocol)
* **Dominio:** `ITLA_E2` | **Password:** `Cisco123` | **Versión:** 2
* **Servidor:** SWM1
* **Clientes:** SWM2, SWAC1, SWAC2, SWAC3
* **Modo Transparente:** SWA13 y SW (VLANs creadas manualmente ya que el enrutador R2 segmenta el dominio VTP).

### 2. Spanning Tree Protocol (Rapid-PVST+)
* **SWM1:** Raíz Primaria (Primary Root).
* **SWM2:** Raíz Secundaria (Secondary Root).
* **Portfast:** Habilitado en todos los puertos de acceso conectados a hosts finales.
* **Convergencia:** Optimizada a 1-2 segundos.

### 3. EtherChannel (LACP 802.3ad)
* **Enlaces:** Puertos `e0/1`, `e0/2`, `e0/3` agrupados entre SWM1 y SWM2 (`Port-channel 1`).
* **Negociación:** Modo `active` configurado en ambos extremos.

### 4. HSRP (Hot Standby Router Protocol)
* **Versión 2**, con `preempt` habilitado en todos los nodos.
* **Gateway:** La IP virtual actúa como puerta de enlace para todos los hosts de la sede principal.

| Dispositivo | Prioridad | Rol |
| :--- | :--- | :--- |
| **R1-Core** | 110 | Activo |
| **SWM1** | 90 | Standby 1 |
| **SWM2** | 80 | Standby 2 |

### 5. Duplex
* **Configuración:** Modo `full-duplex` forzado manualmente en todas las interfaces de la topología para evitar *mismatches*.

### 6. OSPFv2 (Open Shortest Path First)
| Parámetro | Valor |
| :--- | :--- |
| **Proceso** | 1 |
| **Router-ID R1-Core**| 1.1.1.1 |
| **Router-ID R2** | 2.2.2.2 |
| **Área 0 (Backbone)**| R1-Core y enlace P2P |
| **Área 1 (Stub)** | Redes detrás de R2 |
| **ABR** | R1-Core |
| **Ruta Predeterminada**| Propagada vía `default-information originate always` |

---

##  Políticas de Control de Acceso
* Autenticación local requerida (usuario: `admin`) para acceso por Consola y líneas VTY (Telnet).
* Contraseña `enable secret` configurada en todos los equipos de red.
* Temporizador `exec-timeout` establecido en 10 minutos para prevenir sesiones huérfanas.
* Cifrado de contraseñas de texto plano activo mediante `service password-encryption`.
* Mensaje *Banner MOTD* configurado advirtiendo sobre acceso restringido.

---

##  Pruebas de Conectividad (Ping)

Se verificó el enrutamiento extremo a extremo desde cada host simulado.

**Salida ICMP Exitosa Esperada:**
```bash
84 bytes from X.X.X.X icmp_seq=1 ttl=255 time=0.512 ms
84 bytes from X.X.X.X icmp_seq=2 ttl=255 time=0.498 ms
84 bytes from X.X.X.X icmp_seq=3 ttl=255 time=0.501 ms
