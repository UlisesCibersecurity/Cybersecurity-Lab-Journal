# Informe de Incidente: Análisis de Tráfico DNS Anómalo

**Herramientas:** Splunk (SIEM), Wireshark (Packet Analysis).
**Escenario:** Detección de exfiltración de datos simulada mediante DNS Tunneling.

## 1. Detección (Splunk)
El SIEM generó una alerta por un volumen inusual de peticiones DNS desde un único host interno. Para investigar, utilicé una consulta SPL (Search Processing Language) específica para aislar el tráfico.

**Query de investigación:**
splunk
index=network_logs sourcetype=stream:dns
| stats count by query_type, src_ip
| where count > 1000
| sort - count

Hallazgo Crítico: La IP interna 10.0.2.15 realizó más de 5000 consultas DNS en un intervalo de 60 segundos hacia un dominio desconocido, lo cual es un indicador claro de compromiso.

##2. Análisis Forense (Wireshark)
Se extrajo la captura de paquetes (PCAP) correspondiente al incidente para examinar el payload de las peticiones.

Filtro aplicado: ip.src == 10.0.2.15 && dns

Evidencia encontrada: Los paquetes DNS contenían subdominios inusualmente largos y codificados en Base64 (ej: data_x8921.attacker-site.com). Esto confirma que el atacante estaba utilizando el protocolo DNS para sacar información confidencial (Exfiltración), evadiendo el firewall tradicional que permite el tráfico puerto 53.

##3. Respuesta y Contención (NIST Framework)
Siguiendo el ciclo de vida de respuesta a incidentes:

Contención: Se aisló el host 10.0.2.15 de la red corporativa mediante segmentación.

Erradicación: Se bloqueó el dominio atacante a nivel de DNS Sinkhole.

Recuperación: Se procedió al reimaged (restauración) del equipo afectado desde una copia de seguridad limpia.
