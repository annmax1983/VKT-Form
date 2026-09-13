# vkt-form — Captura y autocompletado de formularios

[English](../README.md) | [中文](README_zh.md) | Español | [Deutsch](README_de.md) | [日本語](README_ja.md) | [Français](README_fr.md)

Una extensión para el navegador que guarda capturas de formularios web y los rellena automáticamente después. Todos los datos se almacenan localmente, sin subida a la nube.

## Funcionalidades

- **Recopilación de formularios con un clic** — Escanea y guarda todos los campos de formulario de cualquier página
- **Detección profunda** — Incluye Shadow DOM, iframes y editores de texto enriquecido, y también los pasos ocultos de un formulario por etapas
- **Autocompletado consciente del framework** — Escribe mediante el setter nativo y el propio flujo de edición del navegador, así que los componentes controlados de Vue / React / Angular sí actualizan su estado
- **Verificación posterior** — Cada campo se lee de vuelta tras escribirlo; los fallos se muestran en lugar de ignorarse
- **Calibración de campos** — Vincula un campo a un elemento de la página una vez y se rellenará siempre
- **Completamente local** — Todos los datos se almacenan en `chrome.storage.local`, nunca se suben
- **Exportar/Importar** — Respaldo y restauración JSON
- **Nivel gratuito** — 5 capturas, 20 rellenos/día; Premium elimina todos los límites

## Cómo funciona

1. Visita cualquier página con formularios, haz clic en **Recopilar** para escanear y guardar
2. Vuelve a la página más tarde, haz clic en **Rellenar** para autocompletar todos los campos
3. Gestiona las capturas en el panel lateral (rellenar una específica, actualizar, calibrar, eliminar)

## Coincidencia de campos

Cada campo guardado se puntúa contra todos los campos de la página y gana el mejor candidato que supera el umbral de confianza. Lo que queda por debajo se informa como no encontrado, en lugar de escribirse en la casilla equivocada.

Señales, en orden aproximado de peso:

- Atributos `name`, `id` y `autocomplete`
- Texto de la etiqueta, `aria-label`, `<label>` envolvente, placeholder, texto cercano
- Token semántico (`username`, `phone`, `email`, `address`, …) en chino e inglés
- Ruta estructural y posición de fila/columna dentro de contenedores repetidos (filas de tabla)
- Orden DOM, como último recurso

Como la coincidencia se basa en descriptores y no en posiciones, la captura sigue funcionando aunque el sitio cambie el nombre de los campos, reordene el formulario o lo sirva desde otra URL.

## Rellenado

Cada campo se escribe con estrategias escalonadas y el valor se lee de vuelta después de cada intento:

1. **Setter nativo + eventos** — escribe a través del setter de `HTMLInputElement.prototype` y dispara `beforeinput` / `input` / `change`. Pasar por el prototipo es lo que hace que el seguimiento de cambios de React se active.
2. **Disparadores de confirmación** — `blur` / `focusout` para componentes que solo guardan al perder el foco.
3. **Flujo de edición real** — `document.execCommand('insertText')` tras enfocar y seleccionar, que produce eventos indistinguibles de teclear. También es como se rellenan los editores de texto enriquecido.
4. **Adaptador de componentes** — para selects basados en div (Element Plus, Ant Design, Arco, Naive UI, Vant, …) abre la lista como lo haría un usuario y hace clic en la opción con el valor guardado.
5. **Modo depurador** — desactivado por defecto; usa el depurador del navegador para emitir eventos de entrada de confianza en componentes que rechazan todo lo demás. Chrome no permite solicitar este permiso en tiempo de ejecución, así que se concede al instalar: no se usa hasta que lo actives, y se desconecta en cuanto termina el rellenado (mientras está conectado, el navegador muestra un aviso de depuración).

Los campos que siguen sin encontrarse se reintentan durante unos segundos, de modo que los formularios que se renderizan tarde o aparecen de forma condicional también se rellenan.

## Calibración de campos

Las heurísticas cubren la mayoría de las páginas; para el resto existe la calibración. Abre el panel 🎯 de una captura, elige un campo, pulsa **Vincular** y luego haz clic en ese campo de la página. La vinculación se guarda en la captura y siempre tiene prioridad, haga lo que haga el sitio después.

## Normalización de URL

Las capturas se indexan por URL normalizada:
- Se eliminan cadenas de consulta y fragmentos
- El dominio se convierte a minúsculas
- Las barras finales se normalizan
- Opcional: mantener la ruta hash para aplicaciones SPA (interruptor en Configuración)

## Compilación

```bash
npm install
npm run build
```

Salida: `publish/webformkeeper-v{version}.zip`

## Gratis vs Premium

| | Gratis | Premium |
|---|:---:|:---:|
| Capturas | 5 como máximo | Ilimitadas |
| Rellenos por día | 20 | Ilimitados |
| Exportar / Importar JSON | — | ✅ |
| Soporte prioritario | — | ✅ |

## Licencia

Versión gratuita: 5 capturas, 20 rellenos/día. La clave Premium desbloquea el uso ilimitado, la exportación/importación JSON y el soporte prioritario.
