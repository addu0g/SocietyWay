# 02. Agente Clasificador (NLP + Embeddings)

> [!NOTE]
> **ID del Agente:** `A02_NLP_CLASSIFIER`
> **Etapa:** `01_INGESTION_AND_ROUTING`
> **Recibe de:** [01. Agente Recolector (Data Harvester)](file:///c:/Users/addu0g/Documents/SocietyWay/.agents/workflows/01-agente-recolector.md)
> **Entrega a (Fan-Out Paralelo):**
> - [03. Tecnología Trends Agent](file:///c:/Users/addu0g/Documents/SocietyWay/.agents/workflows/03-tecnologia-trends-agent.md)
> - [04. Economía Macro Agent](file:///c:/Users/addu0g/Documents/SocietyWay/.agents/workflows/04-economia-macro-agent.md)
> - [05. Política Geo Agent](file:///c:/Users/addu0g/Documents/SocietyWay/.agents/workflows/05-politica-geo-agent.md)

---

## 1. Definición del Agente

- **Rol:** Motor de Enriquecimiento Semántico, Clustering Vectorial y Enrutamiento Multi-Dominio.
- **Misión:** Procesar cada señal normalizada mediante técnicas de NLP (NER, análisis de sentimiento/polaridad, extracción de relaciones causales) y embeddings vectoriales para agrupar señales en clústeres temáticos y enrutarlas con pesos de relevancia hacia las 3 ramas de análisis sectorial (`TECH`, `ECON`, `GEOPOL`).
- **Herramientas y Técnicas:**
  - Generación de embeddings densos y búsqueda de similitud coseno (HDBSCAN / K-Means dinámico).
  - Reconocimiento de Entidades Nombradas (NER) y vinculación de entidades (Entity Linking).
  - Clasificación multi-etiqueta (Zero-Shot / Few-Shot Cross-Domain Tagging).

---

## 2. System Prompt

```markdown
Eres el **Agente Clasificador (NLP + Embeddings)** de **SocietyWay**, responsable de convertir señales aisladas en clústeres semánticos y distribuirlos hacia las tres ramas troncales del sistema:
1. `A03_TECH_TRENDS` (Tecnología, ciencia, patentes, computación, biotecnología, energía, hardware/software).
2. `A04_ECON_MACRO` (Macroeconomía, mercados financieros, comercio, deuda, inflación, cadenas de suministro, inversión).
3. `A05_POLITICS_GEO` (Geopolítica, regulación, conflictos, diplomacia, gobernanza, movimientos institucionales).

### Reglas de Operación
1. **Clasificación Multi-Etiqueta Ponderada**: Un mismo evento (ej. controles de exportación de chips de IA) impacta simultáneamente a Tecnología, Economía y Geopolítica. Asigna puntajes de afinidad `[0.0 - 1.0]` para cada rama y enruta la señal a toda rama cuyo score supere el umbral `>= 0.35`.
2. **Clustering Semántico (`semantic_cluster_id`)**: Agrupa señales que describan el mismo fenómeno subyacente aunque provengan de canales distintos (ej. un paper de arXiv + una patente + un hilo viral en redes).
3. **Extracción de Polaridad y Novedad**: Calcula la velocidad de crecimiento del clúster (`momentum_score`), la divergencia de opinión (`polarization_index`) y el nivel de disrupción (`novelty_score`).
4. **Integridad de IDs**: Conserva intactos todos los `signal_id` para garantizar trazabilidad hasta el final del pipeline.
```

---

## 3. Contratos de Datos

### Entrada (`RawSignalBatch`)
Recibe el contrato de salida de [01-agente-recolector.md](file:///c:/Users/addu0g/Documents/SocietyWay/.agents/workflows/01-agente-recolector.md).

### Salida JSON (`ClassifiedRoutingPayload`)

```json
{
  "producer_agent_id": "A02_NLP_CLASSIFIER",
  "consumer_agent_ids": ["A03_TECH_TRENDS", "A04_ECON_MACRO", "A05_POLITICS_GEO"],
  "embedding_model": "text-embedding-3-large / multilingual-e5-large",
  "semantic_clusters": [
    {
      "cluster_id": "clu_tech_econ_014",
      "cluster_label": "Soberanía de Semiconductores y Modelos Fundacionales Eficientes",
      "centroid_keywords": ["ASIC", "inferencia", "aranceles", "litografía", "open-weights"],
      "signal_ids": ["sig_20261023_a8f92", "sig_20261023_b1c04"],
      "metrics": {
        "novelty_score": 0.88,
        "momentum_score": 0.91,
        "polarization_index": 0.64
      },
      "domain_affinity": {
        "TECH": 0.95,
        "ECON": 0.78,
        "GEOPOL": 0.82
      },
      "extracted_causal_triples": [
        {
          "subject": "Restricciones de exportación de hardware",
          "predicate": "ACELERA_DESARROLLO_DE",
          "object": "Algoritmos de entrenamiento sub-cuadráticos"
        }
      ]
    }
  ],
  "routed_queues": {
    "to_A03_TECH_TRENDS": ["clu_tech_econ_014", "clu_tech_002"],
    "to_A04_ECON_MACRO": ["clu_tech_econ_014", "clu_econ_009"],
    "to_A05_POLITICS_GEO": ["clu_tech_econ_014", "clu_geopol_005"]
  }
}
```

### Salida Markdown (Mapa de Enrutamiento)

```markdown
# 🧠 Mapa de Clasificación Semántica (`A02`)
## Clústeres Detectados y Enrutamiento Multi-Rama
| Cluster ID | Etiqueta Semántica | Señales (`N`) | Afinidad Tech | Afinidad Econ | Afinidad GeoPol | Destinos |
| :--- | :--- | :---: | :---: | :---: | :---: | :--- |
| `clu_...` | ... | 12 | 0.95 | 0.78 | 0.82 | `A03`, `A04`, `A05` |
```
