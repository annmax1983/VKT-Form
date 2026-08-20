# WebFormKeeper
[English](../README.md) | [中文](README_zh.md) | Español | [Deutsch](README_de.md) | [日本語](README_ja.md) | [Français](README_fr.md)

Extensión de captura y auto-rellenado de formularios web — Guarda con un clic, rellena con un clic. Todos los datos almacenados localmente.

> Basado en Chromium · Manifest V3 · Sin rastreo · Captura manual por el usuario

---

## ¿Por qué WebFormKeeper?

Rellenar los mismos formularios una y otra vez es tedioso. Con WebFormKeeper, guarda una vez y rellena cuando quieras.

| Ventaja | Detalles |
|---------|----------|
| 🔒 **Privacidad** | Todos los datos en el navegador. Sin servidores, sin subidas, sin rastreo. |
| ⚡ **Un clic** | «Recopilar» para guardar, «Rellenar» para restaurar. |
| 🧠 **Coincidencia inteligente** | Atributo name primero, orden DOM como respaldo, compatible con Vue/React. |
| 💾 **Sin pérdida de datos** | Exportación/importación JSON. |
| 🆓 **Gratis** | 5 capturas, 20 rellenados/día. |
| 🌍 **6 idiomas** | Detección automática del idioma del navegador. |

---

## Funcionalidades

| Función | Descripción |
|---------|-------------|
| 📋 **Captura de formularios** | Botón para escanear todos los `input/select/textarea` de la página. |
| ⚡ **Relleno inteligente** | Prioridad: atributo `name`, respaldo: `domIndex + tagName + type`. |
| 🔄 **Compatible con frameworks** | Eventos `input`, `change`, `click`, compatible con Vue/React. |
| 📊 **Gestión de cuotas** | Gratis: 5 capturas, 20/día. Premium: sin límites. |
| 🔑 **Licencia** | Activación Premium desde la página de configuración. |
| 📥📤 **Importar/Exportar** | Backup y restauración JSON. |
| 🌐 **Normalización de URL** | Elimina consultas/fragmentos, minúsculas, hash opcional (SPA). |
| 🌍 **Multi-idioma** | English, 中文, 日本語, Español, Deutsch, Français. |

---

## Navegadores compatibles

| Navegador | Estado |
|-----------|--------|
| Google Chrome | ✅ Completo |
| Microsoft Edge | ✅ Completo |
| Brave | ✅ Soportado |
| Opera | ✅ Soportado |
| Vivaldi | ✅ Soportado |
| Navegadores Chromium | ✅ Soportado (Manifest V3) |

---

## Instalación

### Modo desarrollador

1. Abrir página de extensiones:
   - **Chrome**: `chrome://extensions/`
   - **Edge**: `edge://extensions/`
2. Activar **Modo desarrollador**
3. **Cargar desempaquetado** → seleccionar carpeta `WebFormKeeper`
4. El icono aparece en la barra de herramientas

---

## Uso

### Guardar captura de formulario

1. Visitar página con formulario
2. Clic en el icono de la barra
3. Clic en **🔄 Recopilar**
4. Los campos se escanean y guardan

### Relleno automático

**Método A: Coincidir página actual**
1. Volver a la página guardada
2. Clic en **⚡ Rellenar**
3. Se rellena automáticamente

**Método B: Rellenar captura específica**
1. Buscar el registro en la lista
2. Clic en **Rellenar esta**
3. Se fuerza el rellenado con esa captura

---

## Privacidad

- ✅ **Sin subida de datos** — Almacenamiento en `chrome.storage.local`
- ✅ **Captura manual** — Sin escaneo automático
- ✅ **Sin rastreo** — Sin telemetría ni llamadas remotas
- ✅ **Permisos mínimos** — Solo `storage` y `activeTab`

---

## Permisos

| Permiso | Propósito |
|---------|-----------|
| `storage` | Almacenamiento local de capturas y configuración |
| `activeTab` | Acceso al tab actual solo al hacer clic |

---

## ❤️ Apoyo

Si WebFormKeeper te ayuda, ¡considera apoyarnos!

**[👉 Apoyar WebFormKeeper](https://annmax1983.github.io/WebFormKeeper/)**
