### Cómo auto mount de hdds al plug
#### Pasos
```
## Paso 1: Crear Archivos de Unidad de Montaje en systemd

**Para `/mnt/external_hdd`:**

Crea el archivo `/etc/systemd/system/mnt-external_hdd.mount` con el siguiente contenido:

```ini
[Unit]
Description=Montar External HDD

[Mount]
What=/dev/disk/by-uuid/4670154670153DDD
Where=/mnt/external_hdd
Type=ntfs-3g
Options=defaults

[Install]
WantedBy=multi-user.target

```
```
[Unit]
Description=Montar Koes2 HDD

[Mount]
What=/dev/disk/by-uuid/14EC91C0EC919C94
Where=/mnt/koes2
Type=ntfs-3g
Options=defaults

[Install]
WantedBy=multi-user.target

## Paso 2: Modificar las Reglas de udev

Edita el archivo de reglas de udev en `/etc/udev/rules.d/99-usb-mount.rules` para activar las unidades de montaje de systemd:

ACTION=="add", SUBSYSTEM=="block", ENV{ID_FS_UUID}=="4670154670153DDD", ENV{DEVTYPE}=="partition", TAG+="systemd", ENV{SYSTEMD_WANTS}="mnt-external_hdd.mount"
ACTION=="add", SUBSYSTEM=="block", ENV{ID_FS_UUID}=="14EC91C0EC919C94", ENV{DEVTYPE}=="partition", TAG+="systemd", ENV{SYSTEMD_WANTS}="mnt-koes2.mount"

## Paso 3: Recargar udev y systemd

Ejecuta los siguientes comandos para recargar las reglas y los demonios:
sudo udevadm control --reload-rules
sudo systemctl daemon-reload
## Paso 4: Probar la Configuración

- **Conecta tus discos duros externos.**
- **Verifica los Montajes:** Ejecuta `mount | grep external_hdd` y `mount | grep koes2` para confirmar que los discos están montados.
- **Revisa los Logs:** Utiliza `journalctl -f` para monitorear mensajes del sistema relacionados con el proceso de montaje.
```

#### Explicación de Por Qué Funciona Esta Configuración

**Limitaciones de udev al Ejecutar Comandos de Montaje:**

- **Tiempo de Ejecución Restringido:** udev está diseñado para ejecutar comandos rápidos. Si un proceso tarda demasiado, como montar un sistema de archivos, puede ser interrumpido.
- **Entorno Limitado:** Los comandos ejecutados por udev carecen de un entorno completo y pueden no tener los permisos o variables necesarios.
- **Terminación de Procesos:** udev puede finalizar procesos que no concluyen rápidamente, impidiendo que el montaje se complete.

**Ventajas de Utilizar systemd para el Montaje:**

- **Manejo de Procesos Largos:** systemd está diseñado para gestionar servicios y procesos de mayor duración sin restricciones de tiempo estrictas.
- **Integración Eficiente:** Las unidades de systemd se integran mejor con el sistema, ofreciendo mayor control y fiabilidad.
- **Registros Detallados:** systemd proporciona logs extensos, facilitando la detección y solución de problemas.

**Funcionamiento de la Configuración:**

1. **Creación de Unidades de Montaje:** Los archivos `.mount` de systemd especifican qué dispositivo montar, dónde montarlo y cómo hacerlo.
2. **Actualización de Reglas de udev:** Las reglas se ajustan para que, al detectar los discos, udev notifique a systemd mediante `SYSTEMD_WANTS` para que inicie las unidades de montaje.
3. **Cooperación entre udev y systemd:** udev detecta el evento y delega el proceso de montaje a systemd, que maneja la tarea de manera eficiente.
4. **Montaje Efectivo:** systemd realiza el montaje sin las limitaciones de udev, asegurando que el proceso se complete exitosamente.

**Beneficios de Esta Metodología:**

- **Automatización Confiable:** Garantiza que los discos se monten automáticamente al ser conectados, sin intervención manual.
- **Flexibilidad y Control:** Permite ajustar fácilmente las opciones de montaje y agregar configuraciones adicionales si es necesario.
- **Solución de Problemas Simplificada:** Los registros de systemd facilitan la identificación y corrección de cualquier inconveniente.

Esta configuración aprovecha las fortalezas de udev para detectar hardware y las capacidades de systemd para gestionar servicios y montajes, proporcionando una solución robusta y eficiente para automontar tus discos externos al conectarlos
- - - 

lsblk [-f]
	apt update && apt install ntfs-3g \\Para [NTFS](NTFS)
	
 birdmount https://pve.proxmox.com/wiki/Linux_Container#_bind_mount_points
	mp0: /mnt/bindmounts/shared,mp=/shared

Para que se auto mount on boos : 
		nano /etc/fstab 
		
	Por UUIID mejor por si cambia puerto. nofail pue si no lo pilla no falle boot
- - - 
#proxmox 