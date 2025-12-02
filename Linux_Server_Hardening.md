# Auditoría y Hardening de Servidor Linux

**Escenario:** Preparación de un servidor web para producción asegurando el principio de menor privilegio.

## 1. Gestión de Permisos de Archivos (DAC)
Se identificaron archivos sensibles con permisos inseguros (777). Se procedió a restringir el acceso únicamente al propietario.

**Comandos ejecutados:**
```bash
# Verificar permisos actuales
ls -l /home/analyst/project_files

# Cambiar permisos para que solo el dueño pueda leer/escribir (600)
chmod 600 confidential_data.txt

# Cambiar el propietario del archivo al usuario root para evitar modificaciones no autorizadas
sudo chown root:root confidential_data.txt
