# Política de Seguridad de la Información

> Versión: 0.1 — Borrador inicial
> Aplicable a: los 4 cofundadores de Raven Mind
> Última actualización: 2026-06-13

## 1. Propósito

Esta política define los lineamientos mínimos para proteger la información de Raven Mind: credenciales, código fuente, datos de usuarios, documentos internos, comunicaciones y cualquier activo digital o físico de la empresa.

Aplica desde el primer día, incluso antes de lanzar un producto, porque la mayoría de las brechas de seguridad empiezan por descuidos básicos en equipos pequeños.

## 2. Alcance

Cubre toda la información manejada por o para Raven Mind, en cualquier formato (digital o impreso) y en cualquier dispositivo (personal o de la empresa).

## 3. Principios

1. **Confidencialidad**: la información solo la conocen quienes deben conocerla.
2. **Integridad**: la información no se altera de manera no autorizada.
3. **Disponibilidad**: la información está accesible cuando se necesita.
4. **Mínimo privilegio**: cada persona accede solo a lo estrictamente necesario.
5. **Defensa en profundidad**: combinar varias capas (contraseñas, 2FA, backups, cifrado).

## 4. Credenciales y cuentas

### 4.1. Contraseñas

- Mínimo 12 caracteres. Recomendado: passphrase de 4–5 palabras aleatorias.
- Una contraseña distinta por servicio. **Nunca** reutilizar contraseñas entre servicios.
- Usar un **gestor de contraseñas** (1Password, Bitwarden, KeePass, etc.) compartido de forma segura.
- Activar **2FA** en toda cuenta que lo soporte. Preferir app autenticadora (TOTP) o llave de seguridad sobre SMS.

### 4.2. Cuentas corporativas

- Cada servicio crítico (GitHub org, cloud, dominio, email, etc.) debe estar a nombre de la empresa, no de una persona.
- Definir al menos **dos personas** con acceso de recuperación a cada cuenta crítica.
- Las credenciales se guardan en el gestor de contraseñas del equipo. Nunca en archivos planos, chats, emails o notas adhesivas.

### 4.3. Manejo de incidentes

- Si una credencial se filtra (phishing, robo de dispositivo, leak público), se cambia **inmediatamente** y se notifica al equipo.
- Si se detecta acceso no autorizado a una cuenta, se cierra la sesión, se cambia la contraseña, se revisan logs y se documenta el incidente.

## 5. Dispositivos

### 5.1. Equipos personales vs. de la empresa

- Mientras no haya equipos corporativos, se trabaja en equipos personales. Cada cofundador es responsable de mantener su equipo seguro.
- Recomendado: cifrado de disco activado (FileVault, LUKS, BitLocker) y bloqueo de pantalla automático (≤ 5 minutos).

### 5.2. Actualizaciones

- Sistema operativo y software se mantienen actualizados, especialmente parches de seguridad.
- No se instala software no confiable ni se ejecutan archivos adjuntos sospechosos.

### 5.3. Pérdida o robo

- Reportar al equipo en menos de 24 horas.
- Cambiar contraseñas de todas las cuentas accedidas desde el dispositivo.
- Si el dispositivo contenía información sensible de la empresa, evaluar borrado remoto (Find My, MDM, etc.).

## 6. Código fuente y repositorios

- El código se almacena en la **organización de GitHub de Raven Mind**, no en cuentas personales.
- No se sube al repo credenciales, claves API ni secretos. Usar variables de entorno o gestores de secretos.
- Activar **secret scanning** y **push protection** en los repositorios de la organización.
- Las ramas `main` deben estar **protegidas**: no se permite push directo, todo entra por PR.
- El historial de Git no se reescribe en ramas compartidas (`force push` deshabilitado en `main`).

## 7. Datos de usuarios y clientes

> Aplicará cuando lancemos el primer producto.

- Cumplir con la normativa aplicable (Ley 1581 de 2012 en Colombia para datos personales, GDPR si hay usuarios en la UE, etc.).
- Pedir consentimiento explícito antes de recolectar datos personales.
- Minimizar los datos recolectados: solo lo estrictamente necesario.
- No compartir datos de usuarios con terceros sin base legal.
- Definir un procedimiento de eliminación de datos a solicitud del usuario.

## 8. Comunicaciones

- Para temas sensibles (credenciales, datos de usuarios, decisiones estratégicas) usar canales cifrados de extremo a extremo cuando sea posible.
- No discutir temas confidenciales de la empresa en redes sociales, espacios públicos o con personas ajenas al equipo.
- Phishing: si un email parece sospechoso, verificar el remitente, no hacer clic en enlaces dudosos y reportar al equipo.

## 9. Trabajo remoto y espacios públicos

- No trabajar con información sensible en redes WiFi públicas sin VPN.
- Cuidar la privacidad de la pantalla en cafés, coworkings o espacios compartidos.
- Bloquear el equipo al alejarse, aunque sea un momento.

## 10. Backups

- Hacer backup periódico de la información crítica de la empresa (código, documentos, configs).
- Probar la restauración de backups al menos una vez al año.
- Regla 3-2-1 cuando aplique: 3 copias, 2 medios distintos, 1 offsite.

## 11. Incidentes de seguridad

1. **Detección**: cualquier cofundador que detecte un incidente lo reporta de inmediato al equipo.
2. **Contención**: aíslar el sistema o cuenta afectada, cambiar credenciales.
3. **Erradicación**: eliminar la causa (malware, credencial filtrada, etc.).
4. **Recuperación**: restaurar servicio desde backup si fue necesario.
5. **Lecciones aprendidas**: documentar el incidente y actualizar esta política.

## 12. Revisión de esta política

Esta política se revisará:

- Al menos una vez al año.
- Tras cualquier incidente de seguridad.
- Cuando lancemos producto o entremos a producción con datos de usuarios.
- Cuando se incorpore un nuevo miembro al equipo.

<!-- TODO: definir con qué frecuencia exacta y quién es el responsable de la revisión -->
