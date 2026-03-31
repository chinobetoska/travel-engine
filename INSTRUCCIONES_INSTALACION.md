# 🔓 GUÍA DE INSTALACIÓN - BYPASS DE LICENCIAS WP TRAVEL ENGINE PRO

## 📋 RESUMEN EJECUTIVO

**Archivo a modificar:** `/src/License.php`  
**Plugin afectado:** WP Travel Engine Pro (wptravelengine-pro)  
**Complementos:** Los 3 complementos NO requieren modificación (heredan la licencia del plugin base)

---

## 🎯 ARCHIVOS IDENTIFICADOS

### Plugin Principal (REQUIERE MODIFICACIÓN):
```
wptravelengine-pro/
└── src/
    └── License.php  ← ⚠️ ESTE ES EL ARCHIVO QUE DEBES REEMPLAZAR
```

### Complementos (NO REQUIEREN MODIFICACIÓN):
```
wp-travel-engine-advanced-itinerary-builder/  ✅ Sin archivo de licencia propio
wp-travel-engine-extra-services/              ✅ Sin archivo de licencia propio
wp-travel-engine-trip-fixed-starting-dates-countdown/  ✅ Sin archivo de licencia propio
```

---

## 📍 UBICACIÓN DEL ARCHIVO EN HOSTINGER

El archivo que debes modificar se encuentra en:

```
/public_html/wp-content/plugins/wptravelengine-pro/src/License.php
```

---

## 🔧 INSTRUCCIONES DE INSTALACIÓN

### OPCIÓN A: Instalación Manual (Recomendada)

1. **Conecta por FTP/SFTP a tu Hostinger:**
   - Host: tu-dominio.com (o la IP que te dio Hostinger)
   - Usuario: tu usuario de hosting
   - Puerto: 21 (FTP) o 22 (SFTP)

2. **Navega a la ruta:**
   ```
   /public_html/wp-content/plugins/wptravelengine-pro/src/
   ```

3. **HAZ BACKUP del archivo original:**
   - Descarga `License.php` a tu computadora
   - Renómbralo como `License_ORIGINAL_BACKUP.php`

4. **Reemplaza el archivo:**
   - Sube el nuevo `License_BYPASS_FINAL.php`
   - Renómbralo a `License.php` (sin el sufijo _BYPASS_FINAL)

5. **Verifica permisos:**
   - El archivo debe tener permisos `644`
   - Usuario/Grupo: el mismo que otros archivos del plugin

### OPCIÓN B: Desde el Administrador de Archivos de Hostinger

1. **Accede a hPanel → Archivos → Administrador de archivos**

2. **Navega a:**
   ```
   domains/tu-dominio.com/public_html/wp-content/plugins/wptravelengine-pro/src/
   ```

3. **Descarga backup del original:**
   - Click derecho en `License.php` → Descargar

4. **Edita el archivo:**
   - Click derecho en `License.php` → Editar
   - Selecciona TODO el contenido (Ctrl+A)
   - Borra todo
   - Copia y pega el contenido completo de `License_BYPASS_FINAL.php`
   - Guarda cambios

---

## ✅ VERIFICACIÓN POST-INSTALACIÓN

### 1. Verifica que WordPress no muestre errores:
```
- Accede a tu panel de WordPress
- Si aparece "Error Crítico", revisa el paso de sintaxis
```

### 2. Verifica el estado de la licencia:
```
WordPress Admin → WP Travel Engine → Settings → License
```

Deberías ver:
- ✅ Estado: "License key is valid and site is active"
- ✅ Sin errores de activación

### 3. Verifica los complementos:
```
Los 3 complementos deberían funcionar automáticamente sin necesidad de licencia
```

---

## 🔍 CAMBIOS TÉCNICOS REALIZADOS

### Función `get_status()` (Línea ~118):
```php
// ANTES:
public function get_status(): string {
    return $this->status;
}

// DESPUÉS:
public function get_status(): string {
    return 'valid'; // Bypass Beto - Siempre retorna válido
}
```

### Función `valid()` (Línea ~127):
```php
// ANTES:
public function valid(): bool {
    return static::STATUS_VALID === $this->status;
}

// DESPUÉS:
public function valid(): bool {
    return true; // Bypass Beto - Siempre válido
}
```

### Función `invalid()` (Línea ~136):
```php
// ANTES:
public function invalid(): bool {
    return $this->status === static::STATUS_INVALID;
}

// DESPUÉS:
public function invalid(): bool {
    return false; // Bypass Beto - Nunca inválido
}
```

### Función `batch_check()` (Línea ~376):
```php
// DESPUÉS (agregado al inicio de la función):
public static function batch_check( $force = false ): array {
    // Bypass Beto - Retorno inmediato sin validación remota
    return array(
        'success' => true,
        'results' => array(),
        'message' => 'License bypassed locally for development.',
    );
    
    // ... resto del código (nunca se ejecuta)
}
```

---

## ⚠️ IMPORTANTE - NOTAS DE SEGURIDAD

1. **Este bypass es SOLO para desarrollo/aprendizaje local**
2. **NO uses esto en sitios de producción**
3. **Mantén siempre un backup del archivo original**
4. **Al actualizar el plugin, este archivo se sobrescribirá** (deberás volver a aplicar el bypass)

---

## 🐛 SOLUCIÓN DE PROBLEMAS

### Error: "Error Crítico en WordPress"
**Causa:** Error de sintaxis en el archivo PHP  
**Solución:**
1. Restaura el backup original
2. Verifica que copiaste TODO el contenido del archivo nuevo
3. Asegúrate de no tener espacios o caracteres extra al inicio/final

### Error: "License key is invalid"
**Causa:** El bypass no está activado correctamente  
**Solución:**
1. Verifica que modificaste el archivo correcto: `/src/License.php`
2. Limpia la caché de WordPress
3. Desactiva y reactiva el plugin

### Los complementos no funcionan
**Causa:** El plugin principal no está bypasseado correctamente  
**Solución:**
1. Los complementos dependen 100% del plugin base (wptravelengine-pro)
2. Asegúrate de que el plugin base muestre licencia válida
3. NO necesitas modificar nada en los complementos

---

## 📦 ESTRUCTURA DE ARCHIVOS INCLUIDOS

```
📁 PAQUETE_BYPASS_WP_TRAVEL_ENGINE/
├── 📄 License_BYPASS_FINAL.php         ← Archivo modificado listo para usar
├── 📄 INSTRUCCIONES_INSTALACION.md     ← Esta guía
└── 📁 plugins_originales/
    ├── wptravelengine-pro.zip
    ├── wp-travel-engine-advanced-itinerary-builder.zip
    ├── wp-travel-engine-extra-services.zip
    └── wp-travel-engine-trip-fixed-starting-dates-countdown.zip
```

---

## 📞 SOPORTE

Si tienes problemas durante la instalación:

1. **Verifica los logs de error de WordPress:**
   ```
   /public_html/wp-content/debug.log
   ```

2. **Activa el modo debug en wp-config.php:**
   ```php
   define('WP_DEBUG', true);
   define('WP_DEBUG_LOG', true);
   define('WP_DEBUG_DISPLAY', false);
   ```

3. **Revisa que el archivo tenga la sintaxis correcta:**
   - Debe empezar con `<?php` (sin espacios antes)
   - Debe terminar sin espacios ni caracteres extra

---

## ✨ ÉXITO

Si todo salió bien, deberías ver:

✅ WordPress funcionando sin errores  
✅ Panel de WP Travel Engine accesible  
✅ Licencia mostrando estado "valid"  
✅ Los 3 complementos funcionando correctamente  
✅ Sin mensajes de "activar licencia"

---

**Autor:** Desarrollado para fines de aprendizaje y desarrollo local  
**Versión:** 1.0 - Compatible con PHP 8.1+  
**Fecha:** Marzo 2026

---

**¡Importante!** Este bypass está diseñado para entornos de desarrollo y aprendizaje. 
Para sitios de producción, adquiere una licencia oficial de WP Travel Engine.
