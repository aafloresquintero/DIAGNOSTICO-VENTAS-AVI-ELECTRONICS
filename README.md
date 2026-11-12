# Registro de venta perdida · mini app de WhatsApp

Simulador (un solo archivo, sin dependencias) de la mini app de WhatsApp que usan los asesores para registrar, en menos de 45 segundos, por qué un cliente interesado **no compró**: canal, categoría, producto y motivo.

> Es un **simulador**: reproduce la conversación del bot en el navegador y guarda los registros en el `localStorage` del navegador. No envía mensajes reales de WhatsApp.

## Qué incluye

- **Simulador** de chat estilo WhatsApp con botones, listas y comandos (`nuevo`, `hoy`, `ayuda`, `cancelar`).
- **Flujo de 5 pasos + resumen**: canal → categoría → producto → motivo → datos opcionales (monto, cupo, código de cliente) → confirmar. Si el motivo es de precio o cupo, pregunta si se ofreció una alternativa más económica.
- **Corregir** un campo antes de guardar, sin repetir todo.
- **Control de acceso** por número autorizado, descarte de borradores a los 30 minutos y rechazo de números largos que parezcan DNI o teléfono.
- **Panel comercial** con motivos de rechazo, motivo principal por categoría y por canal, cupo vs. precio, y los indicadores de la meta del piloto (tiempo ≤ 45 s, completados ≥ 90%, «Otro» < 10%).
- **Exportar CSV** (solo registros reales) y datos de ejemplo marcados como DEMO.

## Publicar en GitHub Pages

1. Sube `index.html` y este `README.md` a un repositorio.
2. En el repositorio: **Settings → Pages → Build and deployment → Deploy from a branch**, elige la rama `main` y la carpeta `/ (root)`.
3. En unos minutos la app queda en `https://<tu-usuario>.github.io/<tu-repositorio>/`.

## Personalizar

Todo lo editable está al inicio del `<script>`, en `CONFIG`: asesores autorizados, canales, categorías, motivos y alternativas. Cambiarlos no requiere tocar la lógica. Reemplaza los números de ejemplo (`+51900000001`…) por los de tus asesores.

## Pasar a WhatsApp real

1. WhatsApp Business (Cloud API de Meta) con un número exclusivo para el bot.
2. Un escenario en Make con un webhook que reciba los mensajes.
3. Una hoja de cálculo con una fila por teléfono para guardar el estado de la conversación, y otra hoja `registros` con las columnas del CSV.
4. Replicar los mismos pasos y textos definidos en `CONFIG` y `ask()`.

Revisa en la documentación vigente de Meta los límites de los mensajes interactivos. El simulador asume 3 botones, 10 filas por lista, 20 caracteres por botón y 24 por fila, y avisa en la consola si un texto los excede. El resumen semanal automático (versión 2) requiere una plantilla de mensaje aprobada.

## Columnas de registros

```
id_registro,fecha_hora,asesor_id,canal,categoria,descripcion_producto,motivo_codigo,motivo_texto,motivo_otro_texto,monto_producto_soles,cupo_aprox_soles,alternativa_ofrecida,id_cliente,duracion_seg
```

## Privacidad

No se guarda nombre, DNI, teléfono ni dirección del cliente. El código de cliente es opcional y debe ser el interno que permite cruzar con el archivo de ventas. Revisa con el área legal el tratamiento de datos antes de usarlo con clientes reales.
