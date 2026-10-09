---
type: arquitectura
date: 2026-10-08
status: current
---

# Arquitectura técnica, base de datos y stack del MVP

Hecho con el skill de CTO el 2026-10-08. Marcas: 📌 dicho por Emilia o con fuente · 🔍 inferencia · ❓ pendiente.

## Restricciones (confirmadas por Emilia)

- 📌 Trabaja sola, no programa, presupuesto cero y el plazo del curso.
- 📌 Para el curso se construye solo la landing; el bot de WhatsApp queda para si el negocio sigue. Se simula como si fuera un negocio real, siempre con costo cero o muy bajo.
- 🔍 Hay datos personales desde el primer día (nombre, email, WhatsApp): consentimiento obligatorio y la planilla nunca es pública.

## Decisión: los leads se guardan en una Google Sheet, vía Google Apps Script

**Por qué:** con el alcance del curso (solo la landing), la planilla es lo más simple de armar, de ver y de mantener sin saber programar, y cuesta cero. Emilia ya trabaja en planillas, y la del grupo de validación vive en el mismo lugar.

**Descartadas:**

- **Supabase:** es la mejor base para el producto completo (jugadores, partidos, niveles) y evita migrar después, pero suma una herramienta nueva, reglas de seguridad para configurar y una pausa por inactividad en el plan gratis (📌 se pausa tras una semana sin actividad). No se justifica para una landing. Se reconsidera si se construye el bot.
- **Google Forms o Tally:** cero código, pero no deja construir la landing propia, que es el objetivo del curso.

**Deuda declarada:**

- **Migración:** si se construye el bot, los leads se migran a una base de datos real (Supabase u otra). Es una exportación de CSV más una importación, de bajo costo mientras haya pocos cientos de filas.
- **Spam:** la dirección del script es pública. Mitigación mínima: un campo trampa invisible y validación de datos en el script. Si aparece basura, se suma un captcha.
- **Cuotas de Apps Script:** 📌 cada ejecución dura como máximo unos 6 minutos y hay cuotas diarias por cuenta. Sobra para el volumen esperado; se vigila el registro de ejecuciones.

## Arquitectura

```
Landing estática (HTML + CSS + JS)
  alojada en GitHub Pages (gratis; el repo del código tiene que ser público)
     │
     │  el formulario envía los datos (POST)
     ▼
Google Apps Script publicado como aplicación web
  valida los campos, descarta lo que llena el campo trampa,
  y agrega una fila con bloqueo para que dos envíos no se pisen
     │
     ▼
Google Sheet privada "Leads" (solo Emilia tiene acceso)
     │
     └── Emilia invita a los leads al grupo de WhatsApp de validación
```

**Por qué GitHub Pages:** Emilia ya tiene cuenta de GitHub y el código ya vive ahí; no suma otra cuenta. Descartada Vercel, que es igual de gratis pero agrega otro servicio. El código de la landing no tiene secretos (la dirección del script es pública por diseño), así que el repo público no expone nada.

## Base de datos: hoja "Leads"

| Columna | Tipo | Obligatoria | Para qué |
|---|---|---|---|
| `fecha` | fecha y hora (la pone el script) | Sí | Cuándo se registró |
| `nombre` | texto | Sí | Contacto |
| `email` | texto | Sí | Contacto; el script avisa si se repite |
| `whatsapp` | texto | Sí | Canal del producto y del grupo de validación |
| `categoria_declarada` | número del 1 al 8 | Sí | 📌 La categoría que el jugador dice tener. Es la línea de base para comparar después con el nivel validado y medir cuánta gente juega en la categoría que debería |
| `partidos_desparejos_mes` | número (0, 1, 2, 3 o más) | No | 🔍 Cuántos partidos desparejos le tocaron en el último mes: valida el problema con datos propios |
| `fuente` | texto | No | De dónde vino: club, Instagram, boca a boca (se toma de la URL si viene marcada) |
| `consentimiento` | sí/no | Sí | Sin consentimiento no se guarda |

## Stack

| Pieza | Herramienta | Costo |
|---|---|---|
| Landing | HTML, CSS y JavaScript simples, sin framework | 0 |
| Alojamiento | GitHub Pages | 0 |
| Recepción del formulario | Google Apps Script (aplicación web) | 0 |
| Almacenamiento de leads | Google Sheet privada | 0 |
| Construcción | Claude Code | Incluido en el plan de Emilia |

## Si sigue el negocio: el bot (no entra en el curso)

- 📌 Desde el 15-01-2026, Meta prohíbe en WhatsApp los asistentes de IA de uso general y permite bots de un servicio concreto (reservas, soporte, notificaciones). 🔍 Un bot que arma partidos de pádel encaja en esa lógica, aunque la fuente no lo nombra.
- 📌 Desde el 01-10-2026, Meta cobra también los mensajes de servicio: el bot deja de ser costo cero.
- ❓ No se pudo confirmar que un bot participe en grupos: se diseña para conversaciones uno a uno.
- 🔍 Al construirlo, los datos pasan a una base real con tablas de jugadores, parejas, partidos y confirmaciones de nivel.

## Próximos pasos verificables

1. Crear un repo aparte para el código de la landing. **Prueba:** el repo existe en GitHub y GitHub Pages muestra una página.
2. Crear la Google Sheet y el Apps Script. **Prueba:** un envío de prueba agrega una fila con fecha.
3. Construir la landing (problema, solución y formulario) y conectarla. **Prueba:** Emilia se registra desde el celular y ve su fila en la planilla.

## Fuentes

- [Automation Atlas — Supabase free tier limits 2026](https://automationatlas.io/answers/supabase-free-tier-limits-2026/)
- [respond.io — WhatsApp chatbot policy 2026](https://respond.io/blog/whatsapp-chatbot-policy-2026)
- [Achiya Automation — WhatsApp Cloud API 2026 update](https://achiya-automation.com/en/blog/whatsapp-cloud-api-2026-update/)
- [Better Sheets — Apps Script quotas](https://bettersheets.co/explained/quota)
