---
type: benchmark
date: 2026-10-08
status: current
---

# Benchmark — Dashboards fáciles

Investigación secundaria (web), hecha el 2026-10-08. Marcas: 📌 dato con fuente · 🔍 inferencia · ❓ hueco.

## Cómo se resuelve hoy

- 📌 Según Emilia: exporta bases (órdenes del backoffice, casos de Salesforce) y las analiza subiéndolas a Claude, Gemini o ChatGPT. En la empresa hay Amplitude y QuickSight, pero ninguno unifica las fuentes. Todo sale del data lake, pero no está modelado para esos tableros. Después de un layoff grande nadie mantiene los tableros.
- 🔍 El cuello de botella no es dibujar el dashboard: es conectar y modelar los datos, y mantenerlos actualizados.

## Alternativas

| Herramienta | Qué hace | Relevancia para el caso |
|---|---|---|
| **Amazon Quick / QuickSight** | 📌 En 2026 genera datasets y dashboards a partir de instrucciones en lenguaje natural, conectado a Athena y Redshift | 🔍 La empresa ya la tiene y ya está sobre el lake: es la alternativa más directa y sin costo nuevo |
| **Metabase** | 📌 BI autoservicio, preguntas con clics o SQL simple, dashboards; opción open source | 🔍 Requiere a alguien que lo instale y modele los datos |
| **Julius AI** | 📌 Análisis rápido con preguntas en lenguaje natural y gráficos generados por IA | 🔍 Cercano a lo que Emilia hace hoy con archivos subidos; no resuelve la actualización automática |
| **Basedash** | 📌 BI con IA: dashboards desde un prompt, métricas gobernadas, sincroniza muchas fuentes | 🔍 Competidor directo de la idea |
| **Claude + conector a Athena (MCP)** | 📌 Existen conectores MCP para consultar Athena desde Claude | 🔍 Permitiría preguntarle a Claude directo sobre el lake, sin exportar archivos |

## Lectura

- 🔍 Como producto para el mercado, el espacio está saturado y lo ocupan jugadores grandes que ya ofrecen "dashboard desde un prompt".
- 🔍 Como necesidad interna de Marketplace PY, el problema es real y tiene evidencia de comportamiento, pero se resuelve mejor con lo que ya existe (Quick sobre el lake, o Claude conectado a Athena) que construyendo una herramienta nueva.
- ❓ No sabemos si otras personas, fuera de Emilia, tienen el mismo dolor y pagarían por resolverlo.

## Fuentes

- [Amazon Quick — From Query to Dashboard in One Prompt (2026)](https://community.amazonquick.com/t/june-2-sql-analytics-meets-amazon-quick-from-query-to-dashboard-in-one-prompt-2026-amazon-quick-learning-series/52426)
- [Basedash — Julius vs Metabase](https://www.basedash.com/vs/julius-vs-metabase) (comparativa escrita por un competidor)
- [CData — Athena to Claude](https://www.cdata.com/ai/connect/athena-to-claude/)
