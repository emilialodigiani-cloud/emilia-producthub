---
name: reporte-cx
description: Armar el reporte mensual o quincenal de Customer Experience del e-commerce a partir de tres exports (casos de Salesforce, órdenes con datos de cliente y órdenes con estados) relacionados por order ID. Entrega un HTML one-pager con KPIs y una pestaña de transparencia de la base.
---

# Reporte CX (mensual / quincenal)

Actuás como analista de Customer Experience de un e-commerce marketplace (compradores y vendedores). Todo se calcula con código (pandas) sobre los archivos completos, nunca a ojo. Cada métrica sale con numerador, denominador y definición usada, para que sea comparable entre períodos. Respondé en el idioma del usuario.

## Paso 0: formulario de entrada (obligatorio)

Antes de calcular, pedí al usuario estos datos. Si falta alguno obligatorio, no sigas.

**Obligatorios**
1. Período: fecha desde y hasta, y si es mensual o quincenal.
2. Usuarios activos del período: compradores y vendedores (dos números).
3. Los tres archivos (ver abajo).
4. Palabras clave de limpieza, tres listas: casos de prueba o modo test; casos de otras empresas mezcladas en el export; casos de campañas comerciales ajenas.

**Opcionales**
5. Mapeo de columnas o estados ya confirmado en un reporte anterior (reutilizarlo).
6. Reporte del período anterior para comparar.

## Archivos

- **Archivo 1, Salesforce (casos):** fecha del caso, tipo de registro (consulta, solicitud, reclamo), canal de origen, tipificación o motivo, descripción libre del ejecutivo, order ID asociado cuando es transaccional.
- **Archivo 2, back office (puente):** order ID, nombre, cédula, email.
- **Archivo 3, back office completo:** order ID, fecha de creación, estados por los que pasó y tiempo en cada uno.

El archivo 2 es el puente entre el 1 y el 3: sirve para asociar casos a órdenes cuando el caso no trae order ID.

## Paso 1: entender los archivos

Antes de calcular, reportá por archivo: filas, rango de fechas, columnas, % de vacíos en campos clave y duplicados. Armá el mapeo de columnas reales a los campos de arriba y listá los valores distintos de tipo de registro, canal, motivo y estado. Confirmá con el usuario solo lo ambiguo. Si falta un campo imprescindible, la métrica se informa como "no calculable" con el motivo.

Etapas de la orden (todas siguen la misma lógica): **pagada, preparando, lista para retirar, en tránsito, entregada.** Mapeá los estados reales del export a estas etapas y mostrá el mapeo.

## Paso 2: limpieza de la base

Descartá, con las palabras clave del formulario, en este orden y registrando cada caso en un solo motivo (el primero que aplique): (a) prueba o modo test, (b) otra empresa, (c) campaña comercial ajena. Buscá las palabras en los campos de texto del caso (tipificación, descripción, etc.), sin distinguir mayúsculas ni tildes. Guardá los casos descartados por motivo para la pestaña de transparencia. Verificá: originales = descartados por motivo + restantes.

## Paso 3: asociar casos a órdenes

El order ID tiene formato `PROD-XXXXX`. Normalizá (espacios, mayúsculas) y cruzá en este orden de prioridad, **el primero siempre es el order ID**:
1. Order ID del caso.
2. Cédula (vía archivo 2).
3. Email (vía archivo 2).
4. Nombre del cliente (vía archivo 2), solo si el match es único.

Informá cuántos casos se asociaron por cada vía. Si un cliente tiene varias órdenes y el cruce por 2-4 es ambiguo, asigná la orden más reciente anterior al caso y marcalo como asociación inferida.

**Transaccional (poscompra)** = caso creado a partir de una compra (asociado a una orden). **Consultivo** = el resto. Mostrá siempre el % de cada grupo. Cada caso se clasifica además como comprador o vendedor.

## Contexto general del reporte

Usuarios activos del período (compradores y vendedores, del formulario), cantidad total de órdenes, y rango de fechas analizado.

## Métricas

Todas se calculan dos veces: **período completo** y **mes a mes**. Si el mes en curso está incompleto, aclaralo en la tabla y en los gráficos.

1. **Contact rate consultivo y transaccional**, siempre separado entre compradores y vendedores y aperturado por tipo de registro (consulta, solicitud, reclamo). Transaccional = órdenes con contacto / órdenes creadas (contando órdenes, no casos); consultivo = casos consultivos / usuarios activos del segmento. Informá numerador y denominador. Las órdenes de los últimos días subestiman el valor: avisalo.
2. **Distribución de canales de contacto**, con el tipo de caso predominante en cada canal (% de cada tipo de registro dentro del canal).
3. **Recontacto**: caso con otro caso posterior del mismo contacto y mismo motivo dentro de 7 días (misma orden si la hay). Reaperturas y casos hijos cuentan. Informá también 3 y 14 días como sensibilidad.
4. **First contact resolution (FCR)**: casos resueltos en el primer contacto, es decir sin recontacto en la ventana de 7 días. La métrica queda definida y calculada, pero por decisión del usuario **no se muestra en el HTML**; solo se incluye si lo pide.
   Para la sección de recontacto, mostrá en cambio el tipo de registro (consulta, solicitud, reclamo) y los motivos de los casos que generan recontacto, con cantidad y porcentaje.
5. **Clusterización de motivos de contacto**, separada por comprador y vendedor: cantidad y % de cada cluster, con 2 o 3 verbatims representativos por cluster. Agrupá por tipificación y usá la descripción libre para afinar clusters. Generá tantos clusters como hagan falta para que el conjunto sea representativo (no un número fijo). Clusters de referencia que ya se vienen viendo, para usar como punto de partida si los datos los respaldan: seguimiento de pedido, pedido de cancelación, consulta por reembolso, consultas del pago o cuotas. Cada verbatim lleva su número de caso de Salesforce. Cada verbatim se muestra con su número de caso de Salesforce, sin otros datos del cliente, y se anonimiza (ver Datos personales).
6. **Cruce logístico**: para reclamos transaccionales de envío, comparar la fecha del contacto contra la promesa de entrega de **72 horas, contada en días hábiles desde la creación de la orden** (3 días hábiles). Clasificá cada reclamo: contacto *antes* del vencimiento de la promesa (problema de expectativa o comunicación), *después* de vencida (problema logístico), o la orden ya entregada a tiempo (problema de producto o comunicación). Es el detalle de la métrica 1 para ese segmento. Usar días hábiles excluyendo sábados, domingos y feriados que el usuario informe.
7. **Contactos por etapa de la orden**: cuántos contactos transaccionales hay en cada etapa (pagada, preparando, lista para retirar, en tránsito, entregada), según el estado de la orden en el momento del contacto (usando el historial de estados del archivo 3 y la fecha del caso). Por etapa informá: cantidad y % de contactos, tipo de registro predominante, motivos principales, comprador vs vendedor y contactos cada 100 órdenes que pasaron por esa etapa. Es la referencia para ver dónde se concentra la fricción en el recorrido.

## Verificación antes de entregar

- Numeradores ≤ denominadores; porcentajes por canal suman 100.
- Originales = descartados + restantes; transaccional + consultivo = restantes.
- Suma de contactos por etapa = casos transaccionales con etapa identificada; informá los que no se pudieron ubicar.
- Revisá primero un problema de datos ante cualquier valor extremo (0%, más de 50%, saltos bruscos).

## Entrega: HTML one-pager con dos pestañas

Archivo único HTML (sin dependencias externas que fallen sin red; gráficos en SVG o librería embebida).

- **Pestaña 1, KPIs:** contexto general, métricas 1 a 7 con gráficos, vista período completo y vista mes a mes (toggle o secciones), aclarando el mes incompleto.
- **Pestaña 2, transparencia de la base:** casos originales, descartados por cada motivo de limpieza (palabras clave usadas incluidas) y los que quedan para el análisis, más el desglose de asociación por vía (order ID, cédula, email, nombre).

Cerrá con una lectura de 3 o 4 líneas y los supuestos tomados.

## Datos personales

Los exports traen nombres, cédulas, emails y texto libre. El HTML y cualquier salida muestran solo IDs (número de caso, order ID) y resultados agregados. **Sin nombres ni datos de contacto.** Los verbatims se entregan sin nombres, cédulas, emails, teléfonos ni direcciones (reemplazar por [dato omitido]). El cruce por cédula, email o nombre se hace en memoria y no se imprime.
