# 00. Fuentes de Datos Globales (Data Sources Layer)

> [!NOTE]
> **Capa de Origen (`SRC_GLOBAL_INPUTS`)**
> Define el universo de señales crudas que alimentan al [Agente Recolector](file:///c:/Users/addu0g/Documents/SocietyWay/.agents/workflows/01-agente-recolector.md).

---

## 1. Taxonomía de Fuentes

| Categoría | Identificador | Fuentes de Referencia / APIs | Frecuencia | Señales Clave |
| :--- | :--- | :--- | :--- | :--- |
| **Noticias Globales** | `SRC_NEWS` | Reuters, AP, Bloomberg, GDELT Project, RSS Internacionales | Tiempo real / Horaria | Crisis geopolíticas, anuncios corporativos, cambios regulatorios. |
| **Redes Sociales** | `SRC_SOCIAL` | X/Twitter, Reddit, Hacker News, Bluesky, Substack, Foros técnicos | Continua | Sentimiento público, memes emergentes, debates virales, señales tempranas. |
| **Papers Científicos** | `SRC_PAPERS` | arXiv, bioRxiv, OpenAlex, Europe PMC, PubMed, Nature/Science | Diaria | Avances en IA, biotecnología, física, ciencias sociales computacionales. |
| **Patentes** | `SRC_PATENTS` | USPTO, EPO (Espacenet), WIPO PatentScope, Google Patents | Semanal | I+D industrial, hardware emergente, propiedad intelectual estratégica. |
| **Datos Económicos** | `SRC_ECON` | FRED, Banco Mundial, FMI, OCDE, BLS, Bancos Centrales | Diaria / Mensual | Inflación, tasas de interés, PIB, flujos comerciales, desempleo. |

---

## 2. Contrato de Salida hacia `A01` (`SourceEventStream`)

### Esquema JSON

```json
{
  "stream_batch_id": "src_batch_2026_10_23_01",
  "captured_at": "2026-10-23T22:00:00Z",
  "events": [
    {
      "raw_id": "raw_99481",
      "source_category": "SRC_NEWS | SRC_SOCIAL | SRC_PAPERS | SRC_PATENTS | SRC_ECON",
      "provider_name": "arXiv / FRED / GDELT / etc.",
      "url": "https://...",
      "published_at": "2026-10-23T18:15:00Z",
      "language": "en",
      "region_iso": "GLOBAL",
      "title": "Título original del documento o serie",
      "raw_content": "Texto íntegro, abstract o serie temporal serializada...",
      "metadata": {
        "authors": [],
        "engagement_metrics": {},
        "patent_ipc_codes": [],
        "economic_indicator_code": null
      }
    }
  ]
}
```
