---
type: experiencia
date: 2026-10-13
status: current
---

# Experiencia de coordinar un partido con el bot (y cómo correrla a mano)

Marcas: 📌 dicho por Emilia o con fuente · 🔍 inferencia o propuesta · ❓ pendiente.

🔍 Propuesta de diseño hecha en conversación, no una decisión. No está en el PRD ni en `decisions/`. Sirve de guion para el bot (etapa 2) y, adaptado, para el grupo de validación manual (etapa 1).

## Principios

- 📌 Los partidos ya se arman por WhatsApp (camino del MVP, 2026-10-08).
- 🔍 Conversación uno a uno, con botones y poco texto.
- ❓ Si un bot puede participar en grupos de WhatsApp no se pudo confirmar (arquitectura del MVP, 2026-10-08). Todo el diseño es 1 a 1.
- 🔍 El bot no reserva canchas: el turnero está fuera del alcance (`context/scope.md`).

## Antes del partido

1. **Alta, una sola vez.** El bot pide nombre, categoría declarada, club y días/horarios habituales. Si viene de la landing, esos datos ya existen.
2. **Pedido de rival.** El jugador escribe "busco rival". El bot pregunta día, hora, club y quién es su compañero.
3. **Búsqueda.** El bot propone una pareja disponible:
   > Encontré una pareja para el sábado 19 hs en el Club X. Nivel validado: 6ª, con 4 partidos confirmados. ¿Les sirve?
   > **[Sí] [Ver otra] [No]**
4. **Doble aceptación.** Solo cuando las dos parejas aceptan, el bot pasa los contactos. Nunca se expone el número de un desconocido antes.
5. **Cancha.** La reservan los jugadores y lo marcan: "reservada".
6. **Recordatorio** un día antes con hora, club y rivales.

## Después del partido

7. **Resultado.** A las 2 horas el bot le pregunta a una persona de cada pareja: "¿Cómo salió?". Si las dos respuestas coinciden, el resultado queda cargado.
8. **Pregunta clave.** "Ese nivel que declararon, ¿fue el real?" con tres botones: **[Más bajo] [Igual] [Más alto]**. Se pregunta a cada pareja por separado y a ciegas.
9. **Cierre.** El bot agradece y muestra el nivel actualizado.

## Dónde puede fallar

| Riesgo | Qué hacer |
|---|---|
| 🔍 Los rivales que acaban de perder se vengan en la calificación | El nivel sale de cruzar la respuesta con el marcador (`context/scope.md`). Una calificación que contradice el resultado pesa menos. ❓ La regla exacta de ponderación no está definida. |
| 🔍 Con pocos jugadores en La Plata, la búsqueda casi siempre dirá "no hay rivales" | Por eso existe la etapa 1 manual: se junta masa antes de automatizar. |
| 🔍 La categoría es individual, pero los partidos son de parejas | El nivel sigue a la persona, no a la pareja. ❓ Cómo se combina el nivel de dos jugadores no está definido. |
| 🔍 La gente no contesta | Es el supuesto que invalida el PRD si responde menos de la mitad de los partidos (`context/scope.md`). |
| 📌 Desde el 01-10-2026 Meta cobra también los mensajes de servicio | 🔍 Un partido genera unos 4 a 6 mensajes iniciados por el bot por jugador (recordatorio, resultado, pregunta clave). El costo cero deja de valer para el bot. ❓ El monto exacto no se calculó. |

## Cómo correrlo a mano en la etapa 1

🔍 El mismo guion, con Emilia haciendo el papel del bot. Es una propuesta, no una decisión.

| Paso del bot | Versión manual |
|---|---|
| Alta | Ya está en la Google Sheet de leads. |
| Búsqueda de rival | Emilia mira la planilla y propone parejas por mensaje directo. |
| Doble aceptación | Emilia pasa los contactos solo cuando las dos parejas dijeron que sí. |
| Recordatorio | Mensaje manual un día antes. |
| Resultado y pregunta clave | Mensaje uno a uno a cada pareja; las respuestas se cargan en una pestaña de partidos de la misma planilla. |

Lo que se mide en esta etapa:

- ❓ Qué porcentaje de partidos llega a tener resultado y nivel confirmado (umbral pendiente, ver `context/scope.md`).
- ❓ Si la pareja inferior también vive el partido desparejo como un problema (`context/open-questions.md`).
