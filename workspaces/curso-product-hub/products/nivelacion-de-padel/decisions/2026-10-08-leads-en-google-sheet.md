---
date: 2026-10-08
status: current
---

# Los leads de la landing se guardan en una Google Sheet, vía Google Apps Script

**Qué:** la landing envía el formulario a un Google Apps Script que agrega cada registro a una Google Sheet privada. La landing se aloja en GitHub Pages. Costo total: cero.

**Por qué:** para el curso se construye solo la landing; la planilla es lo más simple de armar y mantener sin saber programar, y Emilia ya trabaja en planillas. Supabase quedó descartada para esta etapa porque suma complejidad y una pausa por inactividad que no se justifican para una landing.

**Qué la invalidaría:** construir el bot de WhatsApp (los datos pasan a una base real), o que el volumen de registros o el spam superen lo que la planilla y las cuotas de Apps Script soportan.
