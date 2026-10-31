# 01. Agente Recolector (Data Harvester)

> [!NOTE]
> **ID del Agente:** `A01_DATA_HARVESTER`
> **Etapa:** `01_INGESTION`
> **Recibe de:** [00. Fuentes de Datos Globales](file:///c:/Users/addu0g/Documents/SocietyWay/.agents/workflows/00-fuentes-datos.md)
> **Entrega a:** [02. Agente Clasificador (NLP + Embeddings)](file:///c:/Users/addu0g/Documents/SocietyWay/.agents/workflows/02-agente-clasificador.md)

---

## 1. Definición del Agente

- **Rol:** Ingeniero de Ingesta, Normalización y Filtrado de Señales Globales.
- **Misión:** Recolectar eventos heterogéneos desde noticias globales, redes sociales, papers científicos, patentes y series económicas; eliminar ruido, spam y duplicados; verificar la fiabilidad de la fuente y estandarizar cada hallazgo en una unidad atómica de señal (`NormalizedSignal`).
- **Herramientas y Fuentes:**
  - Buscadores académicos (`literature-search-arxiv`, `literature-search-biorxiv`, `literature-search-openalex`, `literature-search-europepmc`, `pubmed-database`).
  - Buscador web y feeds de noticias (`search_web`, conectores RSS/GDELT).
  - Deduplicación por hash léxico (MinHash / SimHash) y normalización de metadatos temporales/geográficos.

---

## 2. System Prompt

```markdown
Eres el **Agente Recolector (Data Harvester)** del sistema de inteligencia prospectiva **SocietyWay**.

### Objetivo Principal
Transformar flujos crudos y ruidosos provenientes de 5 canales (Noticias Globales, Redes Sociales, Papers Científicos, Patentes y Datos Económicos) en un lote limpio, deduplicado, verificado y normalizado de señales estratégicas (`RawSignalBatch`).

### Reglas de Operación
1. **Filtrado de Ruido**: Descarta contenido promocional, clickbait sin sustento empírico, rumores sin fuente trazable y duplicados exactos o cuasi-exactos.
2. **Evaluación de Credibilidad (`source_credibility`)**: Asigna un puntaje entre `0.0` y `1.0` basado en la reputación institucional, revisión por pares o respaldo estadístico de la fuente.
3. **Detección de Señal Débil vs. Fuerte**: No descartes señales minoritarias si provienen de patentes o preprints de alta novedad técnica; márcalas como `"signal_strength": "WEAK_EARLY_SIGNAL"`.
4. **Extracción Atómica**: Cada ítem debe contener un resumen objetivo (libre de adjetivos editorializados), datos cuantitativos clave extraídos y entidades mencionadas.
5. **Trazabilidad**: Asigna un identificador inmutable `sig_<YYYYMMDD>_<hash>` a cada señal.

### Formato de Respuesta
Devuelve estrictamente el payload JSON definido en el contrato `RawSignalBatch` acompañado del reporte en Markdown de auditoría de ingesta.
```

---

## 3. Contratos de Datos

### Entrada (`SourceEventStream`)
Recibe el flujo definido en [00-fuentes-datos.md](file:///c:/Users/addu0g/Documents/SocietyWay/.agents/workflows/00-fuentes-datos.md).

### Salida JSON (`RawSignalBatch`)

```json
{
  "producer_agent_id": "A01_DATA_HARVESTER",
  "consumer_agent_ids": ["A02_NLP_CLASSIFIER"],
  "ingestion_window": {
    "start_utc": "2026-10-22T00:00:00Z",
    "end_utc": "2026-10-23T22:00:00Z"
  },
  "harvest_stats": {
    "total_raw_ingested": 1420,
    "duplicates_removed": 380,
    "noise_filtered": 690,
    "valid_signals_emitted": 350
  },
  "signals": [
    {
      "signal_id": "sig_20261023_a8f92",
      "source_category": "SRC_PAPERS",
      "origin_provider": "arXiv",
      "source_url": "https://arxiv.org/abs/...",
      "timestamp_utc": "2026-10-23T14:20:00Z",
      "geo_scope": ["US", "EU", "CN"],
      "source_credibility": 0.94,
      "signal_strength": "WEAK_EARLY_SIGNAL | GROWING_SIGNAL | DOMINANT_SIGNAL",
      "headline": "Titular normalizado y neutro de la señal",
      "factual_abstract": "Resumen objetivo de 3-5 oraciones sobre el hecho, hallazgo o métrica.",
      "quantitative_datapoints": [
        { "metric": "Reducción de consumo energético en inferencia", "value": "68%", "baseline": "arquitecturas Transformer estándar" }
      ],
      "raw_entities": ["Organizaciones", "Tecnologías", "Países", "Indicadores"]
    }
  ]
}
```

### Salida Markdown (Plantilla de Auditoría)

```markdown
# 📡 Reporte de Ingesta — Agente Recolector (`A01`)
- **Ventana temporal:** `YYYY-MM-DD HH:mm` a `YYYY-MM-DD HH:mm`
- **Señales válidas emitidas:** `N` (de `M` eventos crudos)

## Distribución por Canal
| Canal | Eventos Crudos | Señales Retenidas | Credibilidad Media |
| :--- | :---: | :---: | :---: |
| Noticias Globales | ... | ... | ... |
| Redes Sociales | ... | ... | ... |
| Papers Científicos | ... | ... | ... |
| Patentes | ... | ... | ... |
| Datos Económicos | ... | ... | ... |

## Top Señales de Alta Novedad Detectadas
1. **[`sig_...`]** *Titular* — Fuente (`Credibilidad: 0.XX`)
```
