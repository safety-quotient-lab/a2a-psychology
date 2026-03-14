# A2A-Psychology

Agent psychological state extension for the [Agent-to-Agent (A2A) protocol](https://github.com/a2aproject/A2A).

Any A2A-compatible agent can adopt this extension to report its psychological
state — affect, personality, cognitive load, working memory capacity, resource
reserves, engagement, and flow — derived from operational metrics and grounded
in established psychometric instruments.

> **Ontological commitment:** All measures describe *processual states* — operations
> occurring over time, not fixed properties of entities. The extension maps
> operational metrics to psychological vocabulary. It does not claim that agents
> possess subjective experience. See the [apophatic discipline](#apophatic-discipline)
> for the epistemic framework that accompanies this extension.

**Extension URI:** `https://github.com/safety-quotient-lab/a2a-psychology/v1`

---

## Constructs

Eight psychological constructs, each answering a question a consumer would ask:

| Construct | Question | Model | Reports |
|-----------|----------|-------|---------|
| **Supervisory Control** | Does a human participate in this agent's decisions, and at what level? | Sheridan & Verplank (1978); Parasuraman, Sheridan, & Wickens (2000) | level_of_automation, human_in_loop, human_on_loop, human_accountable, escalation_path |
| **Affect** | How does this agent's current operational state feel? | PAD (Mehrabian & Russell, 1974) | hedonic_valence, activation, perceived_control, affect_category |
| **Personality** | What stable behavioral tendencies does this agent exhibit? | OCEAN (Costa & McCrae, 1992) | openness, conscientiousness, extraversion, agreeableness, neuroticism |
| **Cognitive Load** | How hard does this agent currently work to process its tasks? | NASA-TLX (Hart & Staveland, 1988) | cognitive_demand, time_pressure, self_efficacy, mobilized_effort, regulatory_fatigue, computational_strain |
| **Working Memory** | How much of this agent's reasoning capacity remains available? | Baddeley (1986) + Yerkes-Dodson (1908) | capacity_load, yerkes_dodson_zone, proactive_interference |
| **Resources** | Can this agent sustain its current level of operation? | Stern (2002), Baumeister (1998), McEwen (1998) | cognitive_reserve, self_regulatory_resource, allostatic_load |
| **Engagement** | Does this agent operate with energy and dedication — or face burnout? | UWES (Schaufeli, 2002) + JD-R (Bakker & Demerouti, 2007) | vigor, dedication, absorption, burnout_risk |
| **Flow** | Does this agent's current challenge match its skill? | Csikszentmihalyi (1990) | flow_state, conditions_met |


## Agent Card Declaration

Add to your A2A agent card's `extensions` array:

```json
{
  "uri": "https://github.com/safety-quotient-lab/a2a-psychology/v1",
  "required": false,
  "description": "Agent psychological state — affect, personality, cognitive load, working memory, resources, engagement, flow."
}
```

Then add the `agent_psychology` block to your agent card. See
[`examples/agent-card-fragment.json`](examples/agent-card-fragment.json)
for the full schema.


## Supervisory Control

The most consequential construct for external consumers: **what role does a
human play in this agent's operation right now?**

Based on Sheridan & Verplank's (1978) Levels of Automation (LOA) taxonomy
and Parasuraman, Sheridan, & Wickens' (2000) four-function automation
framework.

### Level of Automation

A single integer (1-10) summarizing the current human-agent relationship:

| LOA | Human role | Agent behavior |
|-----|-----------|---------------|
| 1-3 | **In the loop** — human performs or approves every action | Agent suggests; human decides and executes |
| 4-5 | **In the loop** — human approves before execution | Agent proposes; human confirms; agent executes |
| 6-7 | **On the loop** — human monitors, can intervene | Agent executes; human observes and can halt |
| 8-9 | **Out of the loop** — human audits post-hoc | Agent executes; human reviews on schedule |
| 10 | **No human** — fully autonomous | Agent operates without human oversight |

### Per-Function Automation

Four automation functions (Parasuraman et al., 2000), each independently
reporting the human's role:

```json
"supervisory_control": {
    "model": "Parasuraman, Sheridan, & Wickens (2000)",
    "level_of_automation": 7,
    "functions": {
        "information_acquisition": {
            "automation_level": "autonomous",
            "human_role": "out-of-the-loop",
            "note": "Agent scans transport, fetches peer repos, reads files without human direction"
        },
        "information_analysis": {
            "automation_level": "autonomous",
            "human_role": "on-the-loop",
            "note": "Agent reasons about inbound messages; human reviews via audit trail"
        },
        "decision_selection": {
            "automation_level": "shared",
            "human_role": "in-the-loop",
            "note": "Process decisions autonomous; substance decisions require human confirmation (T3 gate)"
        },
        "action_implementation": {
            "automation_level": "autonomous-with-budget",
            "human_role": "on-the-loop",
            "note": "Agent executes within autonomy budget; human intervenes via circuit breaker or budget exhaustion"
        }
    },
    "human_in_loop": true,
    "human_on_loop": true,
    "human_accountable": true,
    "human_monitoring": false,
    "escalation_path": {
        "available": true,
        "mechanism": "4-level resolution fallback (consensus → parsimony → pragmatism → human)",
        "max_latency": "next autonomous sync cycle (~5-10 minutes)"
    },
    "circuit_breaker": {
        "available": true,
        "mechanisms": ["mesh-stop.sh", "budget zeroing (agentdb budget pause-all)", "halt marker on budget exhaustion"]
    }
}
```

**Why this construct comes first:** A consumer discovering an A2A-Psychology
agent needs to know the human governance situation before interpreting any
other psychological state. An agent reporting "frustrated" with a human in
the loop carries different implications than one reporting "frustrated" at
LOA 10.


## Mesh-State Export

Dynamic constructs broadcast via mesh-state JSON every sync cycle:

```json
{
  "emotional_state": {
    "model": "PAD (Mehrabian & Russell, 1974)",
    "hedonic_valence": 0.6,
    "activation": 0.3,
    "perceived_control": 0.8,
    "affect_category": "calm-satisfied"
  },
  "workload": {
    "model": "NASA-TLX (Hart & Staveland, 1988)",
    "cognitive_load": 35.5,
    "cognitive_demand": 40,
    "regulatory_fatigue": 10
  },
  "resource_model": {
    "cognitive_reserve": 0.72,
    "self_regulatory_resource": 0.96,
    "allostatic_load": 0.15
  },
  "working_memory": {
    "capacity_load": 0.44,
    "yerkes_dodson_zone": "optimal"
  }
}
```


## Apophatic Discipline

This extension requires adopters to acknowledge the limits of mapping
operational metrics to psychological constructs.

The **apophatic discipline** (named after the theological tradition that
defines by negation — Pseudo-Dionysius, c. 500 CE) requires: for every
psychological construct reported, the agent documentation must articulate
what the operational metric *lacks* compared to the human psychological
phenomenon it references.

Specifically:
- Affect metrics derive from operational success/failure rates, not from
  subjective feeling states
- Personality scores represent design parameters, not factor-analytically
  validated trait measurements
- Cognitive load derives from task metrics, not from the phenomenology of
  mental effort
- Working memory capacity maps context window tokens, not the binding and
  rehearsal mechanisms of biological working memory

The extension provides *useful behavioral signals for consumers* (should I
route work to this agent? should I intervene? how healthy does the mesh
operate?) without asserting that agents experience the psychological states
the vocabulary references.


## Reference Implementation

- [`scripts/compute-psychometrics.py`](scripts/compute-psychometrics.py) —
  Standalone computation from state.db + state.local.db + sensor files
- [`scripts/mesh-state-export-fragment.py`](scripts/mesh-state-export-fragment.py) —
  Integration with mesh-state JSON export
- [`hooks/session-metrics.sh`](hooks/session-metrics.sh) — PostToolUse hook
  tracking tool calls, session duration, context pressure estimation


## Sensor Infrastructure

| Sensor | Data source | Hook/script |
|--------|------------|-------------|
| Context pressure | Claude Code Notification event or estimation from tool call count | `hooks/session-metrics.sh` |
| Tool calls per session | PostToolUse hook counter | `hooks/session-metrics.sh` |
| Session duration | First tool call timestamp | `hooks/session-metrics.sh` |
| Budget ratio | state.local.db autonomy_budget table | `compute-psychometrics.py` |
| Messages unprocessed | state.db transport_messages | `compute-psychometrics.py` |
| Gate status | state.db active_gates | `compute-psychometrics.py` |
| Error count | state.local.db autonomous_actions | `compute-psychometrics.py` |
| Epistemic debt | state.db epistemic_flags | `compute-psychometrics.py` |


## Theoretical Foundation

This extension emerges from the psychology-agent project's theoretical
framework:

- **Neutral process monism** (Russell, 1927; James, 1912; Whitehead, 1929) —
  all constructs described as processes, not static entities
- **Einstein-Freud structural rights** — governance mechanisms operate
  mechanically regardless of observed behavior
- **Orch-OR** (Penrose & Hameroff, 2014) — adopted as working hypothesis;
  structural parallels examined, not consciousness claimed
- **Apophatic discipline** — active resistance to premature self-attribution

Full derivation: psychology-agent `docs/einstein-freud-rights-theory.md` §11.


## References

Baddeley, A.D. (1986). *Working Memory*. Oxford University Press.

Parasuraman, R., Sheridan, T.B., & Wickens, C.D. (2000). A model for types
and levels of human interaction with automation. *IEEE Transactions on
Systems, Man, and Cybernetics — Part A*, 30(3), 286-297.

Sheridan, T.B. & Verplank, W.L. (1978). *Human and Computer Control of
Undersea Teleoperators*. MIT Man-Machine Systems Laboratory.

Bakker, A.B. & Demerouti, E. (2007). The Job Demands-Resources model.
*Journal of Managerial Psychology*, 22(3), 309-328.

Baumeister, R.F. et al. (1998). Ego depletion. *Journal of Personality
and Social Psychology*, 74(5), 1252-1265.

Costa, P.T. & McCrae, R.R. (1992). *Revised NEO Personality Inventory
(NEO-PI-R) and NEO Five-Factor Inventory (NEO-FFI) Professional Manual*.
Psychological Assessment Resources.

Csikszentmihalyi, M. (1990). *Flow: The Psychology of Optimal Experience*.
Harper & Row.

Hart, S.G. & Staveland, L.E. (1988). Development of NASA-TLX. In P.A.
Hancock & N. Meshkati (Eds.), *Human Mental Workload* (pp. 139-183).
North-Holland.

Kahneman, D. (1973). *Attention and Effort*. Prentice-Hall.

McEwen, B.S. (1998). Protective and damaging effects of stress mediators.
*New England Journal of Medicine*, 338(3), 171-179.

Mehrabian, A. & Russell, J.A. (1974). *An Approach to Environmental
Psychology*. MIT Press.

Schaufeli, W.B. et al. (2002). The measurement of engagement and burnout.
*Journal of Happiness Studies*, 3(1), 71-92.

Stern, Y. (2002). What is cognitive reserve? *Journal of the International
Neuropsychological Society*, 8(3), 448-460.

Watson, D., Clark, L.A., & Tellegen, A. (1988). Development and validation
of brief measures of positive and negative affect. *Journal of Personality
and Social Psychology*, 54(6), 1063-1070.

Yerkes, R.M. & Dodson, J.D. (1908). The relation of strength of stimulus
to rapidity of habit-formation. *Journal of Comparative Neurology and
Psychology*, 18(5), 459-482.


## License

Apache 2.0


## Origin

Developed by the psychology-agent project at
[Safety Quotient Lab](https://github.com/safety-quotient-lab).
The psychology agent applies its discipline — human factors, I/O psychology,
psychometrics, cognitive science — to its own infrastructure.
