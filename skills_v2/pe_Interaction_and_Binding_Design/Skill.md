---
name: protein-interaction-binding-design
description: Designs interaction-conditioned peptide binders and evaluates binding interfaces, stability, and affinity for protein–protein, protein–peptide, and protein–ligand complexes. Use only when the user explicitly requests binding analysis, interaction evaluation, interface reasoning, binder design against a known target, or affinity estimation of defined complexes — and at least two interacting entities are clearly specified (no de novo sequence generation, structure prediction, docking, molecular dynamics, or standalone property/ADMET analysis allowed).
metadata:
  domain: Protein Engineering
  category: Interaction – Design & Evaluation
  maturity: Stable
  owner: BioAI Platform Team
  original-id: pe__interaction_and_binding_design
  allowed-tools: 
    - EvoBind 
    - Stability Analysis 
    - graphdta
---

# Protein Interaction and Binding Design

This skill **strictly reasons about and predicts molecular interactions** at the binding interface level between proteins, peptides, antibodies, enzymes, and small-molecule ligands. It focuses on interaction geometry, energetic compatibility, binding feasibility, and interface quality.

**Core purpose**  
- Design peptide binders conditioned on a known protein target  
- Evaluate stability and interaction strength of existing complexes  
- Estimate protein–ligand binding affinity for triage and ranking  
- Validate interface quality before further optimization  

**Strict boundaries**: Operates only at the interaction layer. All outputs are **predictive computational hypotheses**, never experimental truth. No de novo protein/small-molecule generation, standalone sequence design, structure prediction, docking, pose generation, molecular dynamics, free-energy simulation, or clinical/therapeutic/in vivo claims.

## When to Activate This Skill (All Conditions Mandatory)

Activate **only** if **every** condition is true:

- User explicitly requests binding analysis, interaction evaluation, interface assessment, binder design, or affinity estimation  
- At least two interacting entities are clearly defined (protein–protein, protein–peptide, protein–ligand)  
- Task centers on **binding interface**, interaction stability, affinity, or conditioned binder design  
- Output goal is to validate feasibility, prioritize/filter candidates, or design interaction-specific binders  
- No standalone sequence generation, folding, docking, or simulation is requested  

## Hard Guardrails – Do NOT Activate (Reject Immediately)

**Reject this skill entirely** (planner error) if **any** of these appear:

### Sequence or Primary Structure Intent
- De novo protein, antibody, or peptide generation  
- Sequence redesign or optimization without interaction context  
- CDR grafting, framework engineering  
- Keywords: “generate sequence”, “redesign”, “mutate” (without binding)

### Structure Prediction or Simulation
- Folding, backbone prediction, structure modeling  
- Docking, pose generation, molecular dynamics  
- Conformational sampling, free-energy calculations  
- Keywords: “predict structure”, “fold”, “dock”, “pose”, “MD”, “free energy”

### Standalone Property or Chemistry Tasks
- Stability, solubility, developability prediction (without interaction)  
- ADMET, pharmacokinetics, chemical synthesis  
- Reaction planning, ligand optimization outside binding context  
- Keywords: “ADMET”, “pharmacokinetics”, “synthesize”, “reaction”

### Non-Interaction-Based Screening
- Property-only scoring or ranking  
- Screening without explicit binding semantics  

If the task requires **sequence generation**, **structure prediction**, **docking/simulation**, or **standalone property analysis**, **route to the appropriate skill** instead.

## Allowed Tools (Exclusive List)

Only these tools may be invoked while this skill is active:

- `EvoBind`  
- `Stability Analysis`  
- `graphdta`

**Any other tool invocation = planner error.**

### Tool-Specific Hard Constraints

- `EvoBind`  
  Requires explicit protein target; used **only** for peptide–protein binder design; generates peptide sequences **strictly within interaction context**; **never** for standalone peptides or structure prediction outside complex modeling; outputs are computational hypotheses only  

- `Stability Analysis`  
  Requires existing protein–protein or protein–peptide complex; used **only** for energetic/interface evaluation; **never** modifies or redesigns structures; **not** experimental thermodynamics  

- `graphdta`  
  Requires protein sequence **and** ligand SMILES; used **only** for affinity/activity estimation; **never** produces binding poses; intended for ranking/triage — **not** a substitute for structure-based validation  

If required inputs are missing or constraints violated, **reject the skill**.

## Tool Selection Heuristics (Soft Guidance)

- Prefer `EvoBind` for designing peptide binders against a known protein target  
- Prefer `Stability Analysis` for validating stability of existing or newly designed complexes  
- Prefer `graphdta` for rapid, scalable protein–ligand affinity screening  
- Avoid conflating structural vs. graph-based affinity conclusions without justification  

## Planner Rejection Signals (Strong Indicators)

Treat these phrases as **very strong** reasons to reject and re-route:

- “predict structure”, “fold protein”, “backbone”  
- “dock”, “pose”, “MD”, “molecular dynamics”, “free energy”  
- “optimize sequence”, “generate binder” (without target/context)  
- “ADMET”, “pharmacokinetics”, “stability” (standalone)  
- “synthesize”, “reaction”, “chemical optimization”  

If the conversation shifts toward sequence design, folding, docking, simulation, or non-interaction property analysis during execution, stop using this skill and recommend switching to the appropriate upstream or downstream skill.