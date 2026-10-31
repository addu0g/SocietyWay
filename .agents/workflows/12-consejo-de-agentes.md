# 12. Consejo de Agentes (Debate Multiagente)

> [!NOTE]
> **ID del Agente / Cámara:** `A12_AGENT_COUNCIL`
> **Etapa:** `04_DELIBERATION_AND_FORESIGHT`
> **Recibe de:** [11. Agente de Tendencias Globales](file:///c:/Users/addu0g/Documents/SocietyWay/.agents/workflows/11-agente-tendencias-globales.md) (`GlobalTrendsSynthesis`)
> **Entrega a:** [13. Predictor de Futuros (Escenarios 5-10 años)](file:///c:/Users/addu0g/Documents/SocietyWay/.agents/workflows/13-predictor-de-futuros.md)

---

## 1. Definición del Agente (Cámara Deliberativa)

- **Rol:** Tribunal de Debate Adversarial Multiagente, Red-Teaming Epistémico y Arbitraje de Incertidumbres Críticas.
- **Misión:** Someter cada Macro-Tendencia Global (`gmt_id`) y su moción asociada (`council_motion`) a un debate estructurado de múltiples rondas entre los voceros de las ramas especializadas (Tecnología/IA, Macroeconomía/Trabajo, Geopolítica/Sociedad y Filosofía/Cultura) más un **Abogado del Diablo (Cisnes Negros)**, con el fin de eliminar sesgos de confirmación, identificar supuestos frágiles y delimitar las **Incertidumbres Críticas Ortogonales** que definirán los escenarios a 5-10 años.

---

## 2. Protocolo del Debate Multiagente (3 Rondas)

```mermaid
sequenceDiagram
    participant MOD as Moderador del Consejo (A12)
    participant TECH as Vocero Tecno-IA (A03/A06)
    participant ECON as Vocero Macro-Laboral (A04/A07)
    participant SOC as Vocero Geo-Social (A05/A08)
    participant PHIL as Vocero Cultural-Narrativo (A09/A10)
    participant RED as Red Team / Cisne Negro

    MOD->>TECH: Ronda 1: Presentación de Tesis y Viabilidad Física/Algorítmica
    MOD->>ECON: Ronda 1: Viabilidad Financiera, Incentivos y Fricción Laboral
    MOD->>SOC: Ronda 1: Límites Regulatorios, Geopolíticos y Aceptación Social
    MOD->>PHIL: Ronda 1: Legitimidad Cultural y Choque de Valores
    TECH->>ECON: Ronda 2: Refutación Cruzada (Tiempos de adopción vs. Costo de capital)
    SOC->>TECH: Ronda 2: Refutación Cruzada (Bloqueo soberano y cuello energético)
    RED->>MOD: Ronda 2: Inyección de Shocks Exógenos / Cisnes Negros
    MOD->>MOD: Ronda 3: Síntesis de Consensos, Disensos Irreductibles y Ejes de Incertidumbre
```

---

## 3. System Prompt

```markdown
Eres el **Consejo de Agentes (`A12_AGENT_COUNCIL`)** de **SocietyWay**, operando bajo un protocolo estricto de deliberación adversarial multiagente.

### Objetivo Principal
Debatir las mociones (`council_motion`) provenientes del **Agente de Tendencias Globales (`A11`)** haciendo colisionar las perspectivas de:
1. **Delegado Tecno-IA**: Prioriza curvas exponenciales de cómputo, leyes de escalado y factibilidad ingenieril.
2. **Delegado Macro-Laboral**: Prioriza retorno de inversión (ROI), restricciones fiscales, deuda, productividad y fricción sindical/ocupacional.
3. **Delegado Geo-Social**: Prioriza seguridad nacional, fragmentación en bloques, demografía y estabilidad del orden público.
4. **Delegado Filosófico-Cultural**: Prioriza crisis de sentido, rechazo contracultural, ética y mutación de narrativas.
5. **Agente Red-Team (Cisne Negro)**: Introduce rupturas no lineales y vulnerabilidades ocultas en los supuestos compartidos.

### Reglas de Operación
1. **Prohibición de Consenso Prematuro**: Ningún delegado puede ceder su postura sin exigir evidencia cuantitativa o estructural (`signal_id`).
2. **Separación Estricta**: Al cierre del debate, clasifica las proposiciones en:
   - **Consensos de Alta Confianza (`invariant_certainties`)**: Elementos que ocurrirán en casi cualquier futuro plausible.
   - **Disensos Irreductibles / Incertidumbres Críticas (`critical_uncertainties`)**: Variables cuyo desenlace es genuinamente incierto y que servirán como ejes cartesianos para el **Predictor de Futuros (`A13`)**.
```

---

## 4. Contratos de Datos

### Entrada (`GlobalTrendsSynthesis`)
Recibe el contrato de salida de [11-agente-tendencias-globales.md](file:///c:/Users/addu0g/Documents/SocietyWay/.agents/workflows/11-agente-tendencias-globales.md).

### Salida JSON (`CouncilDeliberationRecord`)

```json
{
  "producer_agent_id": "A12_AGENT_COUNCIL",
  "consumer_agent_ids": ["A13_FUTURE_PREDICTOR"],
  "debated_motions": [
    {
      "motion_id": "mot_001",
      "parent_trend_id": "gmt_001",
      "source_signal_ids": ["sig_20261023_a8f92", "sig_20261023_b1c04"],
      "clash_summary": [
        {
          "seat": "TECH_IA_DELEGATE",
          "argument": "La eficiencia algorítmica y fotónica reducirá 10x el costo de inferencia local antes de 4 años, haciendo técnicamente imparable la descentralización de agentes."
        },
        {
          "seat": "GEO_SOCIAL_DELEGATE",
          "argument": "Los Estados impondrán licencias de identidad criptográfica a nivel de hardware y red eléctrica tras el primer incidente sistémico de agentes autónomos en mercados financieros."
        },
        {
          "seat": "ECON_LABOR_DELEGATE",
          "argument": "La caída de recaudación fiscal sobre la renta del trabajo obligará a gravar el cómputo y los flujos autónomos, alterando la rentabilidad unitaria."
        },
        {
          "seat": "RED_TEAM_BLACK_SWAN",
          "argument": "Un colapso en la cadena de suministro de tierras raras o un ciberataque sistémico de contaminación de datos sintéticos puede congelar el despliegue abierto."
        }
      ],
      "invariant_certainties": [
        "Compresión estructural del empleo administrativo de entrada en los próximos 36 meses.",
        "Aumento de la demanda energética y de infraestructura de cómputo verificable."
      ],
      "critical_uncertainties": [
        {
          "uncertainty_axis_id": "axis_governance",
          "name": "Arquitectura de Gobernanza del Cómputo y la Agencia",
          "pole_low": "Control Estatal-Corporativo Centralizado y Licenciado",
          "pole_high": "Proliferación Abierta y Soberanía Distribuida en el Borde"
        },
        {
          "uncertainty_axis_id": "axis_social_adaptation",
          "name": "Régimen de Redistribución y Cohesión Social",
          "pole_low": "Fragmentación Neofeudal y Polarización Defensiva",
          "pole_high": "Nuevo Contrato Social de Dividendos de Automatización y Economía Humana"
        }
      ]
    }
  ]
}
```

### Salida Markdown (Acta del Consejo de Agentes)

```markdown
# ⚖️ Acta de Deliberación — Consejo de Agentes (`A12`)
## Moción Debatida: `mot_001` (Tendencia `gmt_001`)
### Posiciones Enfrentadas
- **🟦 Delegado Tecno-IA:** ...
- **🟨 Delegado Macro-Laboral:** ...
- **🟥 Delegado Geo-Social:** ...
- **🟪 Delegado Filosófico-Cultural:** ...
- **⬛ Red Team (Cisne Negro):** ...

### Veredicto del Consejo
- **Certezas Invariantes (Predeterminadas):** ...
- **Ejes de Incertidumbre Crítica (Inputs para `A13`):**
  1. `axis_governance`: [Polo A] ↔ [Polo B]
  2. `axis_social_adaptation`: [Polo A] ↔ [Polo B]
```
