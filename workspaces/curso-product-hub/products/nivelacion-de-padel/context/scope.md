---
updated: 2026-10-08
---

# Alcance

Marcas: 📌 dicho por Emilia o con fuente · 🔍 inferencia · ❓ pendiente.

**Qué entra en la primera versión:** la landing con captura de leads (nombre, email, WhatsApp, categoría declarada, partidos desparejos del último mes, fuente y consentimiento) y el grupo de validación manual. Para el curso se construye solo la landing; el bot queda para si el negocio sigue, siempre con costo cero o muy bajo.

**Zona:** 📌 La Plata, empezando por algunos clubes.

**A quién NO le vende:**

- 📌 **Principiantes:** todavía no tienen categoría ni se enfrentan al problema. Se reconsidera si aparecen pidiendo nivel para empezar a competir.
- 📌 **Federados:** ya tienen ranking, se conocen entre ellos y saben a lo que se exponen. Se reconsidera si el producto llega a torneos.

**Qué NO hace (por ahora):**

- Reservar canchas ni ser turnero. Se reconsidera si la base de jugadores crece y los clubes lo piden.
- Validación de nivel por entrenadores. Se reconsidera una vez que exista una forma de verificar quién es entrenador.
- Calcular el nivel con un algoritmo complejo. En la primera versión, el nivel sale de la confirmación de los rivales cruzada con el resultado.

**Restricciones duras:** 🔍 las reglas de WhatsApp para bots, a verificar con el skill de CTO antes de construir el bot.

**Supuestos y qué los invalidaría:**

- Los jugadores cargan resultados y confirman niveles → se invalida si en el grupo de validación menos de la mitad de los partidos tiene resultado confirmado (❓ umbral a confirmar).
- Los rivales califican con honestidad cuando se cruza con el resultado → se invalida si aparecen calificaciones de revancha o entre amigos que contradicen el marcador.
