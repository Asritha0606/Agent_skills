---
name: small-molecule-generation-optimization
description: Generates de novo small molecules and ligands, performs controlled edits, fragment-based assembly, pharmacophore-guided design, and goal-directed optimization of existing molecules under explicit chemical or property constraints. Use only when the user explicitly requests molecular generation, modification, chemical space exploration, lead optimization, or candidate library creation — and outputs are strictly candidate SMILES or design hypotheses (no standalone property prediction, ADMET scoring, docking, molecular dynamics, retrosynthesis, reaction planning, or experimental/therapeutic claims allowed).
metadata:
  domain: Small Molecule
  category: Generative – Design & Optimization
  maturity: Stable
  owner: ChemAI Platform Team
  original-id: sm__molecular_generation_and_optimization
  allowed-tools: 
    - structurebasedgen 
    - subgen 
    - mergesub 
    - gflowinf 
    - marsinf 
    - pgmg_molgen 
    - rgroup_enumeration 
    - mcts 
    - reinvent 
    - augmented 
    - crem
---

# Small Molecule Generation and Optimization

This skill **strictly generates and optimizes small molecules and ligands** in chemical space. It produces chemically valid candidate structures (SMILES) using de novo, fragment-based, edit-based, pharmacophore-guided, combinatorial, reinforcement-learning, or Monte Carlo strategies under explicit design constraints.

**Core purpose**  
- Create novel molecules from scratch  
- Optimize existing leads through controlled, interpretable edits  
- Explore chemical space with defined objectives  
- Assemble fragment-, scaffold-, or R-group-based candidate libraries  
- Produce design hypotheses for downstream evaluation  

**Strict boundaries**: Purely generative and design-oriented. All outputs are **computational design hypotheses**, never validated leads, drugs, or experimentally confirmed entities. No standalone property prediction, ranking/screening of large libraries, docking, pose scoring, molecular dynamics, free-energy methods, retrosynthesis, reaction planning, or biological/activity claims.

## When to Activate This Skill (All Conditions Mandatory)

Activate **only** if **every** condition is true:

- User explicitly requests molecular generation, modification, optimization, chemical space exploration, or candidate creation  
- Task involves creation/transformation of SMILES or design libraries  
- Output goal is candidate molecules, seeds, or libraries for later screening/docking  
- Any objectives are treated as **design constraints** (not final evaluation criteria)  

## Hard Guardrails – Do NOT Activate (Reject Immediately)

**Reject this skill entirely** (planner error) if **any** of these appear:

### Property Prediction or Screening
- ADMET, toxicity, solubility, permeability, clearance scoring  
- Standalone ranking, filtering, or library screening  
- Keywords: “predict ADMET”, “toxicity”, “rank compounds”, “filter library”

### Interaction or Simulation
- Docking, pose prediction, molecular dynamics  
- Binding affinity estimation, free-energy calculations  
- Keywords: “dock”, “bind”, “pose”, “MD”

### Synthesis or Execution Chemistry
- Retrosynthesis, reaction planning, synthetic route feasibility  
- Experimental protocols or synthesis instructions  
- Keywords: “synthesize”, “reaction”, “retrosynthesis”

### Pure Analysis (Non-Generative)
- Chemical similarity search, clustering, diversity analysis only  

If the task requires **property prediction**, **docking/simulation**, **retrosynthesis**, or **screening without generation**, **route to the appropriate skill** instead.

## Allowed Tools (Exclusive List)

Only these tools may be invoked while this skill is active:

- `structurebasedgen`  
- `subgen`  
- `mergesub`  
- `gflowinf`  
- `marsinf`  
- `pgmg_molgen`  
- `rgroup_enumeration`  
- `mcts`  
- `reinvent`  
- `augmented`  
- `crem`

**Any other tool invocation = planner error.**

### Tool-Specific Hard Constraints

- `structurebasedgen`  
  Requires defined protein structure and binding pocket; generates ligands conditioned on pocket geometry; **never** performs docking or pose scoring; property constraints allowed only as soft design filters  

- `subgen`  
  Operates **only** on existing molecules; produces fragments/rationales; **never** generates full molecules  

- `mergesub`  
  Operates **only** on extracted rationales; outputs composite fragments for seeding; **not** final ligands  

- `gflowinf`  
  Incremental fragment-based generation; may apply property bias; **must** preserve chemical validity at every step  

- `marsinf`  
  Requires reference molecule; performs localized, interpretable edits; **must** preserve core scaffold  

- `pgmg_molgen`  
  Requires explicit pharmacophore constraints; generated molecules **must** satisfy pharmacophore geometry  

- `rgroup_enumeration`  
  Requires fixed scaffold and R-group positions; deterministic combinatorial expansion only; for SAR-style exploration  

- `mcts`  
  Requires single lead; optimizes against explicit reward; **never** interpreted as global screening  

- `reinvent`  
  Policy-driven exploration; reward functions encode design objectives only; outputs are hypothesis candidates  

- `augmented`  
  SMILES augmentation for robust exploration/diversity; **never** claims convergence to optimal molecules  

- `crem`  
  Fragment-level edits around lead; chemistry-driven and deterministic; for interpretable local refinement  

If required inputs are missing or constraints violated, **reject the skill**.

## Tool Selection Heuristics (Soft Guidance)

- Use `reinvent` / `augmented` for broad/global chemical space exploration  
- Use `marsinf` / `crem` for local, interpretable lead optimization  
- Use `gflowinf` for fragment-aware, incremental generation  
- Use `rgroup_enumeration` for systematic SAR-style library expansion  
- Use `structurebasedgen` **only** when pocket geometry is explicitly provided  

## Planner Rejection Signals (Strong Indicators)

Treat these phrases as **very strong** reasons to reject and re-route:

- “dock”, “bind”, “pose”, “MD”, “free energy”  
- “predict ADMET”, “toxicity”, “clearance”, “permeability”  
- “rank compounds”, “filter library”, “screen”  
- “synthesize”, “reaction”, “retrosynthesis”, “route”  

If the conversation shifts toward property prediction, docking, simulation, retrosynthesis, or non-generative screening during execution, stop using this skill and recommend switching to the appropriate analytical, simulation, or synthesis skill.