---
name: small-molecule-docking-dynamics-synergy
description: Performs computational evaluation of small-molecule interactions with biological/chemical targets, including protein–ligand docking, pose generation, binding affinity estimation, molecular dynamics stability analysis, time-dependent behavior, and drug–drug/compound–compound synergy assessment. Use only when the user explicitly requests docking, pose evaluation, affinity estimation, MD simulation, interaction stability/flexibility analysis, or synergy/antagonism modeling — and both ligand and target are defined with no molecule generation, modification, standalone property prediction, library filtering, or experimental claims allowed.
metadata:
  domain: Small Molecule
  category: Simulation & Interaction Evaluation
  type: Predictive – Interaction-Centric
  maturity: Stable
  owner: ChemAI Platform Team
  original-id: sm__docking_dynamics_and_synergy
  allowed-tools:
    - diffdock
    - mds
    - gromacs
    - synergy_dds
---

# Small Molecule Docking, Dynamics, and Synergy

This skill **strictly evaluates molecular interactions** under modeled conditions at the physics- or data-driven layer. It assesses binding geometry, interaction strength, temporal stability, pose plausibility, dynamic behavior, and cooperative/antagonistic effects between small molecules and targets.

**Core purpose**  
- Simulate protein–ligand binding and generate/evaluate poses  
- Estimate binding affinity or interaction strength  
- Model time-dependent stability and flexibility  
- Analyze persistence of key interactions  
- Assess drug–drug or compound–compound synergy/antagonism  

**Strict boundaries**: Purely predictive and interaction-centric. All outputs are **computational hypotheses**, never experimental facts or validated results. No molecule generation, structural modification, standalone property prediction (ADMET/toxicity), library filtering/selection, or decision-making about candidates.

## When to Activate This Skill (All Conditions Mandatory)

Activate **only** if **every** condition is true:

- Task centers on interaction modeling (docking, pose evaluation, affinity estimation, dynamics, stability, or synergy)  
- Ligand and target (protein/structure) are explicitly defined and provided  
- Goal is to understand **how** molecules interact under modeled conditions  
- No generation, modification, filtering, or standalone property analysis is requested  

**Typical activation signals**  
- “dock this ligand”  
- “estimate binding affinity”  
- “simulate stability”  
- “run molecular dynamics”  
- “check synergy between drugs”  
- “compare interaction strength”

## Hard Guardrails – Do NOT Activate (Reject Immediately)

**Reject this skill entirely** (planner error) if **any** of these appear:

### Molecule Creation or Editing
- De novo generation, scaffold modification, optimization  
→ Route to **small-molecule-generation-optimization**

### Property-Only Evaluation
- ADMET, toxicity, solubility, lipophilicity prediction  
→ Route to **small-molecule-property-prediction-modeling**

### Screening or Pruning
- Library filtering, ranking without interaction modeling, threshold elimination  
→ Route to **small-molecule-screening-filtering**

### Static Structural Tasks
- Structure prediction, folding, backbone modeling  
→ Route to appropriate structure modeling skill (if defined)

If interaction modeling is not central, **reject this skill** and route accordingly.

## Allowed Tools (Exclusive List)

Only these tools may be invoked while this skill is active:

- `diffdock`  
- `mds`  
- `gromacs`  
- `synergy_dds`

**Any other tool invocation = planner error.**

### Tool-Specific Hard Constraints

- `diffdock` (Protein–Ligand Docking)  
  Requires both ligand and target structures; used **only** for docking or pose generation/evaluation; **never** for absolute experimental affinity claims, molecular modification, or when no target structure is available  

- `mds` (Molecular Dynamics Simulation)  
  Used **only** for time-dependent interaction behavior or qualitative/comparative stability analysis; **never** for long-timescale converged production runs or when experimental validation is implied  

- `gromacs` (High-Fidelity Molecular Dynamics)  
  Used **only** when high-fidelity MD, parameter-level control, or justified compute cost is explicitly required; **never** for rapid hypothesis screening or when approximate dynamics suffice  

- `synergy_dds` (Drug–Drug Synergy Prediction)  
  Used **only** for explicit drug–drug or compound–compound synergy/antagonism requests (phenotypic/response-based); **never** for physical binding modeling, mechanistic explanations, or causal inference  

If required inputs are missing or constraints violated, **reject the skill**.

## Tool Selection Heuristics (Soft Guidance)

- Prefer `diffdock` for rapid, pose-hypothesis generation and evaluation  
- Prefer `mds` for exploratory or comparative dynamics/stability analysis  
- Use `gromacs` **only** when high fidelity or detailed parameter control is critical  
- Use `synergy_dds` **only** for statistical/phenotypic synergy assessment  

## Planner Rejection Signals (Strong Indicators)

Treat these phrases as **very strong** reasons to reject and re-route:

- “generate”, “optimize”, “design new”  
- “filter”, “select best”, “rank library”  
- “predict ADMET”, “toxicity”, “solubility”  
- “synthesize”, “reaction”, “retrosynthesis”  

If the conversation shifts toward molecule generation, property prediction, screening/pruning, or synthesis during execution, stop using this skill and recommend switching to the appropriate generative, analytical, screening, or chemistry-planning skill.