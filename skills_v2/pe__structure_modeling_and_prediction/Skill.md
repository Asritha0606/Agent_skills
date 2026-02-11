---
name: protein-structure-modeling-prediction
description: Predicts, generates, or models three-dimensional protein structures, including backbone scaffolds, monomeric folding, motif-constrained structures, and pocket identification from sequence, partial structure, or motif inputs. Use only when the user explicitly requests 3D structure prediction, backbone generation, motif scaffolding, folding inference, or binding-site detection — and outputs are strictly structural models (no sequence generation, property evaluation, affinity prediction, docking, molecular dynamics, or functional/therapeutic claims allowed).
metadata:
  domain: Protein Engineering
  category: Predictive – Structure
  maturity: Stable
  owner: BioAI Platform Team
  original-id: pe__structure_modeling_and_prediction
  allowed-tools: 
    - Backbone_Generation 
    - boltz_structure_prediction 
    - Monomer Structure Prediction 
    - Motif Scaffolding 
    - fpocket
---

# Protein Structure Modeling and Prediction

This skill **strictly produces predictive three-dimensional structural models** of proteins or antibodies. It converts sequences, partial structures, motifs, or other geometric inputs into backbone scaffolds, folded monomers, motif-preserving structures, or putative binding pockets.

**Core purpose**  
- Predict folded structures from sequence  
- Generate backbone-only scaffolds  
- Scaffold motifs while preserving geometry  
- Identify potential binding pockets on existing structures  
- Prepare structural hypotheses for downstream docking, simulation, or analysis  

**Strict boundaries**: Limited to geometric and spatial modeling. All outputs are **approximate predictive artifacts**, never experimentally validated truth. No sequence design, property scoring, affinity estimation, docking, dynamics, or functional/therapeutic reasoning.

## When to Activate This Skill (All Conditions Mandatory)

Activate **only** if **every** condition is true:

- User explicitly requests 3D structure prediction, modeling, backbone generation, motif scaffolding, folding, or pocket detection  
- Primary output requested is a **structural model**, backbone, scaffold, or pocket map  
- Task focuses on **topology, geometry, or spatial arrangement**  
- Any provided inputs (sequence, motif, partial structure) are used solely to guide structural prediction  
- Goal is to produce a structure for later downstream use (docking, MD, analysis, etc.) — not executed here  

## Hard Guardrails – Do NOT Activate (Reject Immediately)

**Reject this skill entirely** (planner error) if **any** of these appear:

### Sequence-Level Generative Intent
- Sequence generation, redesign, mutation, diversification  
- CDR engineering, residue-level optimization  
- Keywords: “design sequence”, “mutate”, “redesign”, “generate sequence”

### Property or Functional Evaluation
- Stability, solubility, affinity, activity, developability prediction  
- Ranking, scoring, or filtering based on properties  
- Keywords: “predict affinity”, “rank”, “stability”, “solubility”, “activity”

### Interaction or Simulation Modeling
- Docking, pose generation, molecular dynamics  
- Conformational sampling, free-energy calculations  
- Keywords: “dock”, “simulate”, “MD”, “molecular dynamics”

### Ligand or Chemistry-Centric Requests
- Ligand generation or optimization  
- ADMET, medicinal chemistry, reaction planning  
- Keywords: “optimize ligand”, “ADMET”, “synthesis”

If the task requires **sequence generation**, **property analysis**, or **docking/simulation**, **route to the appropriate skill** instead.

## Allowed Tools (Exclusive List)

Only these tools may be invoked while this skill is active:

- `Backbone_Generation`  
- `boltz_structure_prediction`  
- `Monomer Structure Prediction`  
- `Motif Scaffolding`  
- `fpocket`

**Any other tool invocation = planner error.**

### Tool-Specific Hard Constraints

- `Backbone_Generation`  
  Backbone-only scaffolds; **must not** assign amino acid identities or trigger sequence design  

- `boltz_structure_prediction`  
  Accepts valid biological inputs (protein sequence, nucleic acid, ligand); strictly for structure inference; **never** for refinement or simulation; treat output as predictive only  

- `Monomer Structure Prediction`  
  Single-chain proteins only; **not** for multimers or complexes; preserve any confidence metrics  

- `Motif Scaffolding`  
  Requires explicit motif (residue indices or fragment); **must preserve** motif geometry; surrounding scaffold may vary; **never** for functional/affinity optimization  

- `fpocket`  
  Requires an existing protein structure; strictly for binding-site/pocket detection; **never** interpreted as affinity prediction or used to trigger docking/ligand evaluation  

If required inputs are missing or constraints violated, **reject the skill**.

## Tool Selection Heuristics (Soft Guidance)

- Prefer `Monomer Structure Prediction` for standard single-chain folding from sequence  
- Use `Backbone_Generation` when amino acid identities are intentionally undefined  
- Apply `Motif Scaffolding` only when motif geometry preservation is required  
- Use `fpocket` **only** after a structure has been generated or provided  
- Use `boltz_structure_prediction` for broader input compatibility (including ligands/nucleic acids)  

## Planner Rejection Signals (Strong Indicators)

Treat these phrases as **very strong** reasons to reject and re-route:

- “design a sequence”, “mutate residues”, “redesign sequence”  
- “predict affinity”, “rank binders”, “binding strength”  
- “dock”, “simulate”, “MD”, “molecular dynamics”  
- “stability”, “solubility”, “activity”, “developability”  
- “optimize ligand”, “ADMET”, “synthesis”  

If the conversation shifts toward sequence design, property evaluation, or interaction modeling during execution, stop using this skill and recommend switching to the appropriate generative, analytical, or simulation skill.