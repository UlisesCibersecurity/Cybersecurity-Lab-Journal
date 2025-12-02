# Auditoría y Hardening de Servidor Linux

**Proyecto:** Aseguramiento de un servidor previo a producción.
**Habilidades:** Gestión de permisos (DAC), Usuarios y Firewalls.

## 1. Diagnóstico de Permisos Inseguros
Durante la auditoría inicial, identifiqué archivos críticos con permisos `777` (lectura, escritura y ejecución para todos), lo cual representa una vulnerabilidad crítica.

**Acción correctiva:**
Apliqué el principio de menor privilegio restringiendo el acceso únicamente al propietario.

```bash
# Verificar permisos actuales
ls -l /home/analyst/project_files

# Cambiar permisos a 600 (Solo lectura/escritura para el dueño)
chmod 600 confidential_data.txt

# Asegurar la propiedad del archivo al usuario root
sudo chown root:root confidential_data.txt
