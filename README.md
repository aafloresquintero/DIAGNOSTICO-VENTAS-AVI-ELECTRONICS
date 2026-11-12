1. OBJETIVO
Construir un bot de WhatsApp que permita a los asesores registrar, en menos de 45 segundos, cada vez que un cliente interesado NO compra. Debe capturar cuatro datos obligatorios: (a) motivo del rechazo, (b) categoría del producto, (c) descripción simple del producto no comprado y (d) canal de venta. El fin es saber con evidencia por qué se pierde la venta y en qué categorías, para rediseñar el portafolio según el cupo de crédito del cliente.

2. USUARIO
- Usuario principal: asesores de ventas de una empresa aliada a Calidda que vende artefactos y tecnología financiada con CrediCalidda. Trabajan en callcenter, venta directa en zonas Calidda y atención de portal web.
- Contexto de uso: lo usan desde el celular, entre una llamada y otra, de pie o con poco tiempo. Cero capacitación formal; debe entenderse al primer uso.
- Usuario secundario: jefe comercial, que recibe un resumen y consulta la base de datos.
- Idioma: español de Perú, tono cercano y directo.

3. FUNCIONES
F1. Acceso: solo responde a números de asesores autorizados (lista blanca). A los demás, mensaje de "número no autorizado".
F2. Registro guiado en 6 pasos, con botones y listas (el asesor casi no escribe):
   Paso 1. Canal (3 botones): Callcenter / Web / Venta directa.
   Paso 2. Categoría (lista de 10): Celulares, Televisores, Refrigeradoras, Cocinas, Lavadoras, Laptops, Terma de paso, Motos, Otros del hogar, Otra categoría.
   Paso 3. Descripción simple del producto no comprado (texto libre corto; ejemplo: "lavadora LG 13 kg"). Validar mínimo 3 caracteres.
   Paso 4. Motivo principal del rechazo (lista de 10):
     1 Precio muy alto
     2 Cupo de crédito no alcanza
     3 Crédito no aprobado
     4 Cuota mensual muy alta
     5 Prefiere otra marca o modelo que no tenemos
     6 Encontró mejor oferta en otro aliado
     7 Sin stock o entrega demorada
     8 Solo cotizaba, no decide aún
     9 No entendió o desconfía de las condiciones
     10 Otro (pedir 1 línea de texto)
   Paso 5. Datos opcionales (omitibles con el botón "Saltar"): monto del producto en soles, cupo aproximado del cliente en soles, código interno del cliente (ID_Cliente) y si se ofreció una alternativa más económica (3 botones: Sí, la aceptó / Sí, la rechazó / No había).
   Paso 6. Resumen y confirmación (3 botones): Guardar / Corregir / Cancelar.
F3. Corrección: "Corregir" permite elegir qué campo cambiar sin repetir todo.
F4. Comandos de texto: "nuevo" (otro registro), "ayuda" (instrucciones de 3 líneas), "hoy" (cuántos registros hizo el asesor hoy), "cancelar".
F5. Recuperación: si el asesor deja un registro a medias más de 30 minutos, se descarta el borrador y se avisa al volver.
F6. Resumen semanal automático (versión 2) al jefe comercial: top 5 motivos por categoría y por canal, y porcentaje de "Cupo no alcanza" frente a "Precio muy alto".
F7. Normalización con IA (versión 2): una vez por semana, un modelo lee las descripciones libres y los "Otro", y los ordena en marca, tipo y capacidad. No interviene en el flujo en vivo.

4. APARIENCIA (experiencia conversacional)
- Mensajes de máximo 2 líneas, con un solo emoji por mensaje.
- Indicar progreso en cada paso: "Paso 2 de 5".
- Textos base:
  Bienvenida: "Hola {nombre}. Registremos una venta perdida. Toma 40 segundos. ¿Por qué canal fue?"
  Paso 2: "¿De qué categoría era el producto?"
  Paso 3: "Descríbelo en pocas palabras. Ej.: lavadora LG 13 kg."
  Paso 4: "¿Cuál fue el motivo principal por el que no compró?"
  Paso 5: "Si lo tienes a la mano, completa esto. Si no, toca Saltar."
  Confirmación: "Listo, registro guardado. Escribe 'nuevo' para otro."
  Error: "No te entendí. Elige una opción de la lista o escribe 'ayuda'."
- Siempre ofrecer botones o listas antes que pedir texto.
- Sin tecnicismos, sin mensajes largos, sin pedir datos personales del cliente.

5. DATOS
Tabla única, una fila por venta perdida. Campos:
- id_registro (autogenerado)
- fecha_hora (automática)
- asesor_id (del número de WhatsApp)
- canal (Callcenter / Web / Venta directa)
- categoria (valor de la lista)
- descripcion_producto (texto)
- motivo_codigo (1 a 10) y motivo_texto
- motivo_otro_texto (solo si eligió 10)
- monto_producto_soles (opcional)
- cupo_aprox_soles (opcional)
- alternativa_ofrecida (opcional: acepto / rechazo / no_habia)
- id_cliente (opcional, código interno que permite cruzar con el archivo de ventas)
No guardar nombre, DNI, teléfono ni dirección del cliente.

6. RESTRICCIONES
- Construir con la opción más simple: WhatsApp Business (Cloud API de Meta) + una automatización tipo Make + una hoja de cálculo como base de datos. Evitar servidores propios y desarrollos a medida en la versión 1.
- Respetar los límites de los mensajes interactivos de WhatsApp (máximo 3 botones por mensaje; listas de hasta 10 opciones; títulos cortos). Verificar los valores vigentes en la documentación de Meta.
- Usar solo datos internos: sin información personal del cliente. Revisar el consentimiento y el tratamiento de datos con el área legal, según la ley peruana de protección de datos.
- Ningún campo obligatorio adicional a los cuatro pedidos (canal, categoría, descripción, motivo).
- Registro completo en un máximo de 6 interacciones y 45 segundos.
- Las listas de categorías y motivos deben poder editarse sin tocar la lógica del bot.
- Fuera del alcance de la versión 1: encuestas directas al cliente, clasificación con IA en vivo, tableros interactivos y integración con el sistema de ventas.

7. CRITERIOS DE ACEPTACIÓN Y PRUEBA
- Un asesor nuevo completa un registro sin instrucciones en menos de 45 segundos.
- Al menos 90% de los registros iniciados se completan.
- Menos de 10% de los motivos caen en "Otro".
- Casos de prueba: (1) registro completo con opcionales; (2) registro saltando opcionales; (3) corrección de la categoría en el resumen; (4) número no autorizado; (5) abandono y retorno a los 30 minutos.
- Piloto sugerido: 2 semanas con 5 asesores de los tres canales. Las metas de arriba son un punto de partida; ajústalas con los resultados del piloto.
