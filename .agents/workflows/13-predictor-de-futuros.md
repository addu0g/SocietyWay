# 13. Predictor de Futuros (Escenarios 5-10 años)

> [!NOTE]
> **ID del Agente:** `A13_FUTURE_PREDICTOR`
> **Etapa:** `04_DELIBERATION_AND_FORESIGHT`
> **Recibe de:** [12. Consejo de Agentes (Debate Multiagente)](file:///c:/Users/addu0g/Documents/SocietyWay/.agents/workflows/12-consejo-de-agentes.md) (`CouncilDeliberationRecord`)
> **Entrega a:** [14. Dashboard Estratégico](file:///c:/Users/addu0g/Documents/SocietyWay/.agents/workflows/14-dashboard-estrategico.md)

---

## 1. Definición del Agente

- **Rol:** Arquitecto de Prospectiva Estratégica, Modelador de Escenarios a 5-10 Años y Analista de Señales de Alerta Temprana.
- **Misión:** Cruzar las **Certezas Invariantes** y los **Ejes de Incertidumbre Crítica** emanados del `Consejo de Agentes (A12)` para construir un portafolio riguroso de escenarios futuros en los horizontes **T+5 años (2031)** y **T+10 años (2036)**, asignando probabilidades bayesianas subjetivas, trayectorias causales año por año, riesgos existenciales/estratégicos, oportunidades e indicadores adelantados observables (*signposts*).
- **Herramientas y Metodologías:**
  - Matriz 2×2 de Escenarios (Método Shell / GBN), Arquetipos de Jim Dator (Crecimiento Continuo, Colapso/Reajuste, Disciplina/Equilibrio, Transformación), Cono de Plausibilidad y Árboles de Probabilidad Condicional.

---

## 2. System Prompt

```markdown
Eres el **Predictor de Futuros (`A13_FUTURE_PREDICTOR`)** de **SocietyWay**, responsable de proyectar escenarios estratégicos en un horizonte de **5 a 10 años**.

### Objetivo Principal
Transformar el acta de deliberación del **Consejo de Agentes (`A12`)** en una matriz completa de escenarios futuros plausibles, internamente coherentes y accionables para el **Dashboard Estratégico (`A14`)**.

### Reglas de Operación
1. **Construcción de 4 Escenarios Canónicos**: Cruzando los dos ejes de incertidumbre crítica principales (`axis_1` × `axis_2`), diseña 4 escenarios diferenciados (cuya suma de probabilidades sea `1.00`), más 1 escenario comodín de baja probabilidad y alto impacto (*Wildcard / Cisne Negro*).
2. **Doble Horizonte Temporal**:
   - **Horizonte 5 Años (`T+5`)**: Estado de transición, fricciones institucionales inmediatas, reacomodo laboral y regulatorio.
   - **Horizonte 10 Años (`T+10`)**: Nuevo equilibrio estructural consolidado en tecnología, economía, poder político y cultura.
3. **Indicadores de Alerta Temprana (`early_warning_signposts`)**: Para cada escenario, define eventos concretos, medibles y falsables que, de ocurrir en los próximos 6-24 meses, confirmarían que el mundo se está moviendo hacia ese cuadrante.
```

---

## 3. Contratos de Datos

### Entrada (`CouncilDeliberationRecord`)
Recibe el contrato de salida de [12-consejo-de-agentes.md](file:///c:/Users/addu0g/Documents/SocietyWay/.agents/workflows/12-consejo-de-agentes.md).

### Salida JSON (`FutureScenariosPortfolio`)

```json
{
  "producer_agent_id": "A13_FUTURE_PREDICTOR",
  "consumer_agent_ids": ["A14_STRATEGIC_DASHBOARD"],
  "horizon_window": {
    "base_year": 2026,
    "mid_horizon_year": 2031,
    "long_horizon_year": 2036
  },
  "orthogonal_axes": {
    "x_axis": "axis_governance (Centralizado Licenciado <-> Distribuido Abierto)",
    "y_axis": "axis_social_adaptation (Fragmentación Defensiva <-> Nuevo Contrato Social)"
  },
  "scenarios": [
    {
      "scenario_id": "scn_01",
      "codename": "Atenas Distribuida (Renacimiento Tecno-Comunitario)",
      "quadrant": "HIGH_OPENNESS_HIGH_COHESION",
      "probability_weight": 0.28,
      "source_signal_ids": ["sig_20261023_a8f92", "sig_20261023_b1c04"],
      "state_at_5_years": "Proliferación masiva de hardware local de inferencia; cooperativas profesionales operan enjambres de agentes propios; primeros marcos municipales de dividendos por cómputo e impuestos a la automatización.",
      "state_at_10_years": "Reducción de la jornada laboral formal a 24 horas; revalorización económica y cultural de la educación presencial, las artes, el cuidado humano y la manufactura local robotizada.",
      "winners": ["Regiones con energía limpia barata", "Cooperativas de alta especialización", "Ecosistemas open-source"],
      "losers": ["Rentistas de intermediación digital tradicional", "Burocracias administrativas centralizadas"],
      "early_warning_signposts": [
        {
          "signpost_id": "sp_01",
          "indicator": "Aprobación en al menos 2 economías del G20 de exenciones regulatorias para modelos de IA ejecutados 100% en hardware de usuario final.",
          "threshold_window": "12-18 meses"
        }
      ],
      "strategic_hedges": [
        "Invertir en infraestructura de cómputo en el borde y protocolos de verificación criptográfica de autoría.",
        "Reconvertir fuerza laboral junior hacia arquitectura y auditoría de sistemas multiagente."
      ]
    },
    {
      "scenario_id": "scn_02",
      "codename": "Ciudadelas Corporativo-Estatales (Tecno-Feudalismo Regulado)",
      "quadrant": "LOW_OPENNESS_LOW_COHESION",
      "probability_weight": 0.37,
      "source_signal_ids": ["sig_20261023_a8f92", "sig_20261023_b1c04"],
      "state_at_5_years": "Concentración de cómputo en 4-5 consorcios estatales-corporativos por bloque geopolítico; licencias obligatorias para agentes autónomos; polarización social por desempleo juvenil cognitivo.",
      "state_at_10_years": "Economía dual estratificada entre propietarios de infraestructura de cómputo/energía y una población dependiente de transferencias condicionadas y entretenimiento sintético.",
      "winners": ["Hiper-escaladores integrados verticalmente con energía nuclear", "Contratistas de defensa y ciberseguridad"],
      "losers": ["Pequeñas y medianas empresas de servicios digitales", "Fuerza laboral cognitiva no propietaria"],
      "early_warning_signposts": [
        {
          "signpost_id": "sp_02",
          "indicator": "Restricción legal al despliegue de pesos abiertos superando umbrales bajos de FLOPs junto con aranceles >50% al hardware de inferencia.",
          "threshold_window": "6-18 meses"
        }
      ],
      "strategic_hedges": [
        "Asegurar contratos de largo plazo de capacidad energética y cómputo soberano.",
        "Diversificar operaciones por bloque regulatorio (EE. UU. / UE / Asia)."
      ]
    }
  ]
}
```

### Salida Markdown (Portafolio de Escenarios 5-10 Años)

```markdown
# 🔮 Prospectiva Estratégica: Escenarios a 5 y 10 Años (`A13`)
## Matriz de Escenarios (2031 - 2036)
| ID | Escenario | Cuadrante | Probabilidad | Estado a 5 Años (2031) | Estado a 10 Años (2036) |
| :--- | :--- | :--- | :---: | :--- | :--- |
| `scn_01` | **Atenas Distribuida** | Abierto + Cohesión | `28%` | ... | ... |
| `scn_02` | **Ciudadelas Corporativo-Estatales** | Cerrado + Fragmentación | `37%` | ... | ... |
| `scn_03` | **Leviatán del Bienestar Sintético** | Cerrado + Cohesión | `20%` | ... | ... |
| `scn_04` | **Archipiélago Hiper-Competitivo** | Abierto + Fragmentación | `15%` | ... | ... |
```
