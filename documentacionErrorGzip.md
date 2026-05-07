# Informe Técnico – Soluciones para permitir el uso de `gzip`

## 1. Contexto y alcance

Se requiere permitir que un usuario (por ejemplo `alvaro` / `PAMDespWrt`) pueda **comprimir archivos utilizando `gzip`**, manteniendo las siguientes restricciones:

- No modificar propietarios (`owner`) ni grupos (`root:root`) de archivos o directorios ya existentes.
- No añadir usuarios a nuevos grupos.
- No utilizar ACLs.
- Evitar accesos interactivos completos como root.
- Mantener el **principio de mínimo privilegio**.
- Entorno con permisos de sistema de ficheros típicos:
  - Directorios: `755`
  - Archivos: `644`

Con estas limitaciones, se han analizado varias soluciones técnicas.

---

## 2. Soluciones analizadas

---

## 2.1 Uso de `sudo -i`

### Descripción
Se concede al usuario la capacidad de ejecutar:

```bash
sudo -i
```

### Funcionamiento

sudo -i inicia un shell de login (normalmente /bin/bash) con el entorno completo del usuario root.
Desde ese shell, el usuario puede ejecutar cualquier comando del sistema, incluido gzip.

### Beneficios

- Implementación inmediata y sencilla.
- Máxima flexibilidad operativa.
- No requiere modificar permisos del sistema de ficheros.

### Riesgos

- Acceso total como root (control absoluto del sistema).
- Alto riesgo de error humano o acciones no autorizadas.
- Muy difícil de auditar y justificar en entornos regulados.
- Incumple el principio de mínimo privilegio.

### Impacto en seguridad

Crítico. Equivale funcionalmente a entregar acceso root completo.
Valoración 
No recomendada en entornos productivos, regulados o bastionados.

## 2.2 Cambio del grupo del directorio /root

### Descripción

Se modifica el grupo del directorio /root para permitir acceso a determinados usuarios.
Funcionamiento

Los usuarios pertenecientes al grupo configurado pueden acceder (cd) a /root.
Desde ese directorio pueden ejecutar herramientas como gzip.

### Beneficios

No requiere sudo ni reglas adicionales.
Funciona directamente a nivel de permisos del sistema de ficheros.
Simplicidad desde el punto de vista técnico.

### Riesgos

/root es un directorio crítico del sistema.
Riesgo de exposición de ficheros sensibles.
Rompe políticas estándar de bastionado.
Normalmente inaceptable en auditorías de seguridad.

### Impacto en seguridad

Alto. Amplía la superficie de exposición del sistema.
Valoración
No recomendada en sistemas productivos o con requisitos de cumplimiento.

### Información adicional

Esta seria una solución buena, si se aplica a carpetas para los usuarios.

## 2.3 Delegación explícita de gzip mediante sudo (sudo granular)

### Descripción

Se concede acceso únicamente al binario gzip mediante sudo, sin acceso a shell ni a otros comandos.

## Funcionamiento

El usuario puede ejecutar únicamente gzip como root.
No puede abrir shells interactivos (bash, sh).
Se recomienda el uso de -c para no modificar archivos originales.

El fichero original permanece intacto.
El fichero comprimido pertenece al usuario.

### Beneficios

Cumple el principio de mínimo privilegio.
No permite acceso interactivo como root.
Acción totalmente auditable (sudo logs).
Reduce drásticamente la superficie de ataque.
Alineado con buenas prácticas de seguridad.

### Riesgos

Requiere definir explícitamente el comando permitido.
Puede necesitar ajustes si se amplía el alcance funcional (por ejemplo incluir tar).

### Impacto en seguridad

Bajo. Riesgo controlado y acotado al binario permitido.

### Valoración

Solución recomendada.

añadir a sudoers esta linea

```bash
alvaro ALL=(root) NOPASSWD: /usr/bin/gzip *
```

y puedes comprimir con gzip de esta forma

```bash
sudo find /root -maxdepth 1 -type f -name 'archivo*' -print0 \
 | sudo xargs -0 gzip -c > ~/prueba.gz
```
