# Auditoría y Hardening de Servidor Linux

**Proyecto:** Aseguramiento de un servidor previo a producción.
**Habilidades:** Gestión de permisos (DAC), Usuarios y Firewalls.

## 1. Diagnóstico de Permisos Inseguros
Durante la auditoría inicial, identifiqué archivos críticos con permisos `777` (lectura, escritura y ejecución para todos), lo cual representa una vulnerabilidad crítica.

**Acción correctiva:**
Apliqué el principio de menor privilegio restringiendo el acceso únicamente al propietario.

BASH
# Verificar permisos actuales
ls -l /home/analyst/project_files

# Cambiar permisos a 600 (Solo lectura/escritura para el dueño)
chmod 600 confidential_data.txt

# Asegurar la propiedad del archivo al usuario root
sudo chown root:root confidential_data.txt

## 2. Gestión de Usuarios y Accesos
Se auditó el archivo /etc/group para detectar configuraciones erróneas en la elevación de privilegios.

**Acción correctiva:**
Se eliminó al usuario 'invitado' del grupo de administradores (sudo) para prevenir cambios no autorizados en el sistema.

BASH
sudo deluser invitado sudo

## 3. Seguridad de Red Firewall
Se configuró UFW (Uncomplicated Firewall) para denegar todo el tráfico entrante por defecto, excepto SSH.

BASH
sudo ufw default deny incoming
sudo ufw allow ssh
sudo ufw enable
