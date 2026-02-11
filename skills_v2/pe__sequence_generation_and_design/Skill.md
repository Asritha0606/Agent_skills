---
name: protein-sequence-generation-design
description: Generates, redesigns, mutates, or diversifies biological sequences (proteins, antibodies, enzymes, peptides) at the primary sequence level, including de novo creation and constraint-guided design from functional, structural, or contextual inputs. Use only when the user explicitly requests sequence generation, redesign, mutation, diversification, or candidate proposal — and outputs are strictly candidate sequences (no property evaluation, scoring, ranking, structure prediction, docking, simulation, or biological claims allowed).
metadata:
  domain: Protein Engineering
  category: Generative – Sequence
  maturity: Stable
  owner: BioAI Platform Team
  original-id: pe__sequence_generation_and_design
  allowed-tools: 
    - Random Antibody Generation 
    - CDR_Generator 
    - Antibody_Backbone_to_Sequence_Generation 
    - Function_Based_Enzyme_Generation 
    - Sequence_based_peptide_generation 
    - Random Controlled Mutagenesis 
    - Unconditional Sampling 
    - Ligand MPNN
---

# Protein Sequence Generation and Design

This skill **strictly generates or modifies biological sequences** at the primary (amino acid) level for proteins, antibodies, enzymes, and peptides. It produces **candidate sequences only** — never evaluates, scores, ranks, predicts properties, simulates structures, or makes biological/therapeutic claims.

**Core purpose**  
- Create de novo sequences  
- Redesign existing sequences under explicit constraints  
- Diversify sequences for discovery or screening  
- Propose candidates for downstream validation, modeling, or analysis  

**Strict boundaries**: Operates only at the sequence-design layer. No stability, affinity, solubility, developability, structure, docking, dynamics, or clinical reasoning.

## When to Activate This Skill (All Conditions Mandatory)

Activate **only** if **every** condition is true:

- User explicitly requests sequence generation, design, redesign, mutation, diversification, or candidate creation  
- Primary output requested is a protein/antibody/enzyme/peptide **sequence**  
- Task is **generative / transformative** (not evaluative or analytical)  
- Any inputs (sequence, structure, function, ligand) are used **only to guide design** — not to analyze or score  
- Goal is to produce candidates for later validation or downstream skills  

## Hard Guardrails – Do NOT Activate (Reject Immediately)

**Reject this skill entirely** (planner error) if **any** of these appear:

### Property Evaluation / Analytical Intent
- Stability, solubility, affinity, antigenicity, toxicity, developability prediction  
- Ranking, scoring, filtering, comparison based on properties  
- Keywords: “predict”, “evaluate”, “analyze”, “assess”, “score”, “rank”, “compare”

### Structural Modeling or Simulation
- Structure prediction, refinement  
- Docking, molecular dynamics, interaction simulation  
- Binding pose or affinity estimation  
- Keywords: “dock”, “simulate”, “MD”, “molecular dynamics”

### Non-Sequence-Centric Requests
- Small-molecule generation / optimization  
- ADMET, logP, MW, hERG, BBB analysis  
- Retrosynthesis, reaction planning  
- Clinical, therapeutic, efficacy, or safety claims  

If the task requires **property prediction**, **structure modeling**, or **simulation**, **route to the appropriate skill** instead.

## Allowed Tools (Exclusive List)

Only these tools may be invoked while this skill is active:

- `Random Antibody Generation`  
- `CDR_Generator`  
- `Antibody_Backbone_to_Sequence_Generation`  
- `Function_Based_Enzyme_Generation`  
- `Sequence_based_peptide_generation`  
- `Random Controlled Mutagenesis`  
- `Unconditional Sampling`  
- `Ligand MPNN`

**Any other tool invocation = planner error.**

### Tool-Specific Hard Constraints

- `Random Antibody Generation`  
  De novo only; no prior sequence/structure required  

- `CDR_Generator`  
  Requires existing antibody sequence; modifications **limited to CDR regions**  

- `Antibody_Backbone_to_Sequence_Generation`  
  Requires user-provided antibody backbone structure; **never** triggers structure prediction  

- `Function_Based_Enzyme_Generation`  
  Requires valid functional description or EC number; no guarantee of catalysis  

- `Sequence_based_peptide_generation`  
  Requires parent protein/antigen sequence; produces **short peptide candidates only**  

- `Random Controlled Mutagenesis`  
  Requires one or more parent sequences; **must respect** protected residues  

- `Unconditional Sampling`  
  Use **only** when user explicitly requests unconstrained exploration  

- `Ligand MPNN`  
  Requires reliable ligand-binding structure/complex; strictly for sequence design — **never** for affinity evaluation  

If required inputs are missing or constraints violated, **reject the skill**.

## Tool Selection Heuristics (Soft Guidance)

- Prefer refinement / constrained tools when a viable parent sequence or structure exists  
- Use unconstrained / de novo tools **only** when explicit exploration is requested  
- Apply structure-conditioned tools **only** when structures are explicitly provided  
- Avoid unnecessary constraints unless user specifies them  

## Planner Rejection Signals (Strong Indicators)

Treat these as **very strong** reasons to reject and re-route:

- “predict”, “evaluate”, “analyze”, “score”, “rank”, “assess”  
- “stability”, “solubility”, “affinity”, “developability”  
- “dock”, “simulate”, “MD”  
- “optimize ADMET”, “clinical”, “therapeutic outcome”, “efficacy”, “safety”  

If the conversation shifts toward evaluation or modeling during execution, stop using this skill and recommend switching to the appropriate analytical or structural skill.