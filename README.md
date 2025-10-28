# Modificaciones Realizadas en los Scripts

## Resumen

Este documento describe las modificaciones realizadas en los scripts de pronóstico de precipitación y modelo IDD.

---

## 📝 Scripts Modificados

### 1. `pronostico_precipitacion_24h_72h.py`

**Ubicación:** `scripts/py/3_pronostico_precipitacion/pronostico_precipitacion_24h_72h.py`

**Cambios Realizados:**

#### Modificación Principal: Deshabilitación de Generación de Mapas JPG

**Líneas afectadas:** 237-255

**Descripción:** Se comentó completamente el bloque de código que generaba los mapas JPG del pronóstico de precipitación a 24h, 48h y 72h.

**Código anterior:**
```python
if estado == 1 and GENERAR_MAPAS:
    print('OK')
    print('Creando mapas de pronostico de precipitacion a 24h, 48h Y 72h. . .')
    try:
        subprocess.call([bat_dir+'mapa_pronostico_precipitacion_24h_72h.BAT'])
        files = ['pronostico_precipitacion24h.jpg','pronostico_precipitacion48h.jpg','pronostico_precipitacion72h.jpg']
        for f in files:
            if not os.path.isfile(temporales_dir+f):
                estado = 0
                break
    except:
        estado = 0
else:
    print('Generación de mapas deshabilitada, se omite la creación de los JPGs.')
```

**Código modificado:**
```python
# Bloque comentado completamente
# if estado == 1 and GENERAR_MAPAS:
#     print('OK')
#     print('Creando mapas de pronostico de precipitacion a 24h, 48h Y 72h. . .')
#     try:
#         subprocess.call([bat_dir+'mapa_pronostico_precipitacion_24h_72h.BAT'])
#         files = ['pronostico_precipitacion24h.jpg','pronostico_precipitacion48h.jpg','pronostico_precipitacion72h.jpg']
#         for f in files:
#             if not pk.path.isfile(temporales_dir+f):
#                 estado = 0
#                 break
#     except:
#         estado = 0
# else:
#     print('Generación de mapas deshabilitada, se omite la creación de los JPGs.')

# Se omite la generación de mapas
print('Generación de mapas deshabilitada, se omite la creación de los JPGs.')
```

**Razón del cambio:** La generación de mapas estaba causando problemas o no era necesaria en este momento, por lo que se optó por omitir completamente esta sección.

**Impacto:** 
- El script ya no ejecutará el archivo BAT `mapa_pronostico_precipitacion_24h_72h.BAT`
- No se generarán los archivos JPG `pronostico_precipitacion24h.jpg`, `pronostico_precipitacion48h.jpg` y `pronostico_precipitacion72h.jpg`
- Los archivos raster (.tif) seguirán generándose normalmente
- El proceso de copia de archivos JPG (líneas 267-273) no fallará porque verifica la existencia de los archivos

---

### 2. `pronostico_precipitacion.py`

**Ubicación:** `scripts/py/3_pronostico_precipitacion/pronostico_precipitacion.py`

**Cambios Realizados:**

#### Configuración Pre-existente: Flag para Deshabilitar Mapas

**Línea 37:**
```python
GENERAR_MAPA = False
```

**Descripción:** Este archivo ya cuenta con una bandera `GENERAR_MAPA` configurada en `False`, que deshabilita la generación de mapas JPG. El bloque correspondiente (líneas 204-214) maneja correctamente esta configuración.

**Estado:** No se realizaron modificaciones adicionales en este archivo.

---

### 3. `modelo_idd.py`

**Ubicación:** `scripts/py/4_modelo_idd/modelo_idd.py`

**Cambios Realizados:**

No se realizaron modificaciones en este archivo durante esta sesión.

**Estado:** El archivo se mantiene sin cambios.

---

## 🔍 Análisis de la Configuración de Mapas

### Variable `GENERAR_MAPAS` / `GENERAR_MAPA`

Ambos scripts (`pronostico_precipitacion.py` y `pronostico_precipitacion_24h_72h.py`) tienen una variable para controlar la generación de mapas:

- **`pronostico_precipitacion.py`:** Usa `GENERAR_MAPA = False`
- **`pronostico_precipitacion_24h_72h.py`:** Usa `GENERAR_MAPAS = False`

En ambos casos, estas variables están configuradas como `False`, lo que significa que la generación de mapas está deshabilitada por defecto.

---

## 📂 Archivos Afectados

### Rasters (NO afectados)
- ✅ `pronostico_precipitacion.tif` -br generado normalmente en `pronostico_precipitacion.py`
- ✅ `pronostico_precipitacion24h.tif` - generado normalmente en `pronostico_precipitacion_24h_72h.py`
- ✅ `pronostico_precipitacion48h.tif` - generado normalmente en `pronostico_precipitacion_24h_72h.py`
- ✅ `pronostico_precipitacion72h.tif` - generado normalmente en `pronostico_precipitacion_24h_72h.py`

### Mapas JPG (Afectados)
- ❌ `pronostico_precipitacion.jpg` - NO se genera (deshabilitado)
- ❌ `pronostico_precipitacion24h.jpg` - NO se genera (bloque comentado)
- ❌ `pronostico_precipitacion48h.jpg` - NO se genera (bloque comentado)
- ❌ `pronostico_precipitacion72h.jpg` - NO se genera (bloque comentado)

---

## ⚙️ Configuración de Directorios

### Directorios comentados en `pronostico_precipitacion_24h_72h.py`

En las líneas 26, se puede observar que el directorio `bat_dir` está comentado:

```python
#bat_dir = r'/home/cirros/deslizamientos/3_pronostico_precipitacion/scripts/bat/'
```

Esto está relacionado con la deshabilitación de la generación de mapas, ya que este directorio contiene los scripts BAT necesarios para crear los mapas JPG.

---

## 🔄 Revertir Cambios

Si en el futuro se desea habilitar nuevamente la generación de mapas en `pronostico_precipitacion_24h_72h.py`:

1. Descomentar el bloque de código en las líneas 239-251
2. Eliminar las líneas 254-255 (el print simple)
3. Asegurarse de que `bat_dir` esté correctamente definido (descomentar línea 26)
4. Opcionalmente cambiar `GENERAR_MAPAS = True` en la línea 53

---

## 📅 Fecha de Modificación

Modificaciones realizadas: Octubre 2025

---
