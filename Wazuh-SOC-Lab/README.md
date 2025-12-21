## Proyecto 1: Implementación de un SOC Lab con Wazuh SIEM
Descripción: Despliegue de un entorno de monitorización centralizado para detectar y alertar sobre actividades maliciosas en tiempo real.

### **Arquitectura del Lab**:
 -SIEM Manager: Wazuh v4.x (OVA) configurado con IP estática en Red Interna.
 -Endpoint: Windows 10 con Agente Wazuh instalado para telemetría profunda.
 -Atacante: Kali Linux configurado en el mismo segmento de red para pruebas de intrusión.

Escenario 1: Detección de Ataque de Fuerza Bruta (SSH)
Acción: Se realizó un ataque de diccionario contra el servicio SSH del Manager usando la herramienta Hydra desde Kali.

Resultado: El SIEM generó una alerta crítica (Nivel 10) identificando la IP origen del atacante y el usuario objetivo (root).

Log Detectado: Rule ID: 5720 - Multiple authentication failures.

Escenario 2: Monitorización de Persistencia en Windows
Acción: Creación y borrado de cuentas de usuario mediante PowerShell para simular la creación de "puertas traseras".

Análisis Forense: A través del Dashboard de Wazuh, se filtraron los eventos del agente Windows identificando:

Creación: Rule ID: 60109 - User account enabled or created.

Borrado: Rule ID: 60110 - User account deleted.

Valor Técnico: Se logró identificar el campo exacto targetUserName: Atacante dentro del log JSON, demostrando visibilidad total sobre los cambios de privilegios.

🛠️ Desafíos y Troubleshooting (Resolución de problemas)
Durante el despliegue, la máquina OVA no presentaba las credenciales estándar. Utilicé herramientas de línea de comandos de Linux (find, grep) para localizar los scripts de gestión del Indexer en /usr/share/wazuh-indexer/ y procedí a resetear manualmente la contraseña del administrador, asegurando la continuidad del laboratorio.
