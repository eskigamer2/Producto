Credenciales de Acceso
Todos los dispositivos de la red comparten las mismas credenciales de acceso.
Parámetro	Valor
Usuario	admin
Contraseña de usuario	itla123
Enable secret	Cisco123
Exec-timeout	10 minutos
Acceso remoto	Telnet
Acceso local	Consola
Todas las contraseñas están encriptadas mediante service password-encryption. El acceso a cualquier dispositivo requiere autenticación local con el usuario admin.

Mapa de Puertos
Routers
Dispositivo	Puerto	Conectado a	VLANs / Función	Modo
ISP	e0/0	R1-Core e0/2	WAN 203.0.113.1/30	Routed
R1-Core	e0/0	SWM1 e0/0	10,20,30,40,99	Trunk
R1-Core	e0/1	SWM2 e0/0	10,20,30,40,99	Trunk
R1-Core	e0/2	ISP e0/0	WAN 203.0.113.2/30	Routed
R1-Core	s1/0	R2 s1/0	P2P 172.18.10.161/30	Serial
R2	e0/0	SWA13 e0/0	50,60,99	Trunk
R2	e0/1	SW e0/0	70,99	Trunk
R2	s1/0	R1-Core s1/0	P2P 172.18.10.162/30	Serial
Switches de Distribución
Dispositivo	Puerto	Conectado a	VLANs / Función	Modo
SWM1	e0/0	R1-Core e0/0	10,20,30,40,99	Trunk
SWM1	e0/1-3	SWM2 e0/1-3	EtherChannel Po1	Trunk
SWM1	e1/0	SWAC1 e0/0	20,99	Trunk
SWM1	e1/1	SWAC2 e0/0	10,99	Trunk
SWM1	e1/2	SWAC3 e0/0	30,40,99	Trunk
SWM2	e0/0	R1-Core e0/1	10,20,30,40,99	Trunk
SWM2	e0/1-3	SWM1 e0/1-3	EtherChannel Po1	Trunk
SWM2	e1/0	SWAC1 e0/1	20,99	Trunk
SWM2	e1/1	SWAC2 e0/1	10,99	Trunk
SWM2	e1/2	SWAC3 e0/1	30,40,99	Trunk
Switches de Acceso
Dispositivo	Puerto	Conectado a	VLANs / Función	Modo
SWAC1	e0/0	SWM1 e1/0	20,99	Trunk
SWAC1	e0/1	SWM2 e1/0	20,99	Trunk
SWAC1	e1/0	DOC1	20	Access
SWAC2	e0/0	SWM1 e1/1	10,99	Trunk
SWAC2	e0/1	SWM2 e1/1	10,99	Trunk
SWAC2	e2/0	EST1	10	Access
SWAC3	e0/0	SWM1 e1/2	30,40,99	Trunk
SWAC3	e0/1	SWM2 e1/2	30,40,99	Trunk
SWAC3	e3/0	GES1	30	Access
SWA13	e0/0	R2 e0/0	50,60,99	Trunk
SWA13	e1/0	ACA1	60	Access
SWA13	e2/0	FIN1	50	Access
SW	e0/0	R2 e0/1	70,99	Trunk
SW	e0/1	SERVICIOS	70	Access

Tecnologías Implementadas
VTP
•	Dominio: ITLA_E2 | Password: Cisco123 | Versión: 2
•	SWM1 → servidor
•	SWM2, SWAC1, SWAC2, SWAC3 → clientes
•	SWA13 y SW → transparent, VLANs creadas manualmente porque R2 rompe el dominio VTP
Spanning Tree — Rapid-PVST+
•	SWM1 raíz primaria, SWM2 raíz secundaria
•	Portfast en todos los puertos de acceso hacia hosts
•	Convergencia de 1 a 2 segundos
EtherChannel — LACP 802.3ad
•	Puertos e0/1, e0/2, e0/3 entre SWM1 y SWM2 → Port-channel 1
•	Modo active en ambos extremos
HSRP
Dispositivo	Prioridad	Rol
R1-Core	110	Activo
SWM1	90	Standby 1
SWM2	80	Standby 2
•	Versión 2, preempt habilitado en los tres dispositivos
•	IP virtual es el gateway de todos los hosts de la sede principal
Duplex
•	Full duplex forzado en todas las interfaces de todos los dispositivos
OSPFv2
Parámetro	Valor
Proceso	1
R1-Core router-id	1.1.1.1
R2 router-id	2.2.2.2
Área 0	R1-Core + enlace P2P
Área 1 stub	Redes de R2
ABR	R1-Core
Ruta predeterminada	default-information originate always
Control de Acceso
•	Autenticación local con usuario admin en consola y Telnet
•	Enable secret habilitado en todos los dispositivos
•	Exec-timeout de 10 minutos en consola y VTY
•	Contraseñas encriptadas con service password-encryption
•	Banner de acceso restringido en todos los dispositivos

Pruebas de ping por host
DOC1
ping 172.18.8.1
ping 172.18.0.10
ping 172.18.10.70
ping 172.18.9.10
ping 172.18.9.138
ping 172.18.10.10
ping 203.0.113.1
EST1
ping 172.18.0.1
ping 172.18.8.10
ping 172.18.10.70
ping 172.18.9.10
ping 203.0.113.1
GES1
ping 172.18.10.65
ping 172.18.8.10
ping 172.18.0.10
ping 172.18.9.138
ping 172.18.10.10
ping 203.0.113.1
ACA1
ping 172.18.9.1
ping 172.18.9.138
ping 172.18.10.10
ping 172.18.8.10
ping 172.18.0.10
FIN1
ping 172.18.9.129
ping 172.18.9.10
ping 172.18.10.10
ping 172.18.8.10
SERVICIOS
ping 172.18.10.1
ping 172.18.9.10
ping 172.18.9.138
ping 172.18.0.10


Output esperado ping exitoso
84 bytes from 172.18.8.1 icmp_seq=1 ttl=255 time=0.512 ms
84 bytes from 172.18.8.1 icmp_seq=2 ttl=255 time=0.498 ms
84 bytes from 172.18.8.1 icmp_seq=3 ttl=255 time=0.501 ms
84 bytes from 172.18.8.1 icmp_seq=4 ttl=255 time=0.489 ms
84 bytes from 172.18.8.1 icmp_seq=5 ttl=255 time=0.505 ms
Failover HSRP
! Paso 1: estado inicial
show standby brief

! Paso 2: simular caída de R1-Core
interface e0/0
 shutdown

! Paso 3: SWM1 asume como activo
show standby brief

! Paso 4: ping hacia IP virtual
ping 172.18.0.1

! Paso 5: restaurar R1-Core
interface e0/0
 no shutdown

! Paso 6: R1-Core retoma por preempt
show standby brief
Verificación control de acceso
! Probar acceso por consola con credenciales
! Usuario: admin
! Contraseña: itla123
! Enable: Cisco123

! Verificar timeout de sesión
! Dejar inactiva la sesión 10 minutos y verificar desconexión automática
________________________________________

Errores Corregidos
Error	Causa	Solución
VLANs no propagaban a SWA13 y SW	R2 rompe el dominio VTP	Modo transparent con VLANs manuales
Duplex mismatch	Autonegociación en PNETLab	duplex full en todas las interfaces
HSRP sin redundancia real	Un solo dispositivo por grupo	Tres dispositivos: R1-Core, SWM1, SWM2
Overlap en e0/1.70 de R2	IP asignada en e0/0.70 misma red	IP de VLAN 70 únicamente en e0/1.70
e0/2 de R1-Core en DHCP	Sin IP fija hacia ISP	IP fija 203.0.113.2/30 hacia ISP
