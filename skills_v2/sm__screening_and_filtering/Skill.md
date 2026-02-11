---
name: small-molecule-screening-filtering
description: Performs comparative screening, filtering, prioritization, and pruning of existing small-molecule libraries using similarity metrics, pharmacophore matching, substructure rules, novelty assessment, or diversity logic. Use only when the user explicitly requests library reduction, candidate prioritization, rule-based inclusion/exclusion, novelty/diversity enforcement, or preparation of clean sets for downstream workflows — and multiple molecules are provided with no generation, modification, property prediction, docking, simulation, or retrosynthesis allowed.
metadata:
  domain: Small Molecule
  category: Comparative – Selection & Pruning
  maturity: Stable
  owner: ChemAI Platform Team
  original-id: sm__screening_and_filtering
  allowed-tools: 
    - sim_screen 
    - ph_screening 
    - substructure_screen 
    - novelty 
    - tanimoto
---

# Small Molecule Screening and Filtering

This skill **strictly performs comparative, reference-driven, or rule-based selection** across sets of existing small molecules. It reduces library size, enforces constraints, ensures novelty/diversity, and prepares prioritized candidate sets — without ever generating, modifying, optimizing, or simulating molecules.

**Core purpose**  
- Reduce large libraries to manageable size  
- Prioritize candidates by similarity, pharmacophore fit, or structural rules  
- Enforce inclusion/exclusion criteria (e.g., remove toxic substructures)  
- Control novelty and remove redundancy/duplicates  
- Prepare clean, diverse sets for property prediction, docking, or optimization  

**Strict boundaries**: Operates exclusively on existing molecule sets. Outputs determine **which** molecules proceed — never **how** they should be changed. No property estimation, generation, docking, molecular dynamics, retrosynthesis, or biological/efficacy claims.

## When to Activate This Skill (All Conditions Mandatory)

Activate **only** if **every** condition is true:

- Multiple molecules are provided (library, set, or collection)  
- Task is **comparative / relative** (not isolated evaluation)  
- Goal is to keep/discard, rank/prioritize, enforce rules, or control diversity/novelty  
- No structural modification, generation, or simulation is requested  

**Typical activation signals**  
- “filter this library”  
- “screen against known ligands”  
- “apply pharmacophore constraints”  
- “remove toxic substructures”  
- “ensure novelty or diversity”  
- “reduce candidates before docking”

## Hard Guardrails – Do NOT Activate (Reject Immediately)

**Reject this skill entirely** (planner error) if **any** of these appear:

### Property Estimation
- ADMET, toxicity, solubility, permeability prediction  
- Scoring or classification of individual properties  
→ Route to **small-molecule-property-prediction-modeling**

### Molecular Creation or Improvement
- De novo generation, scaffold hopping, lead optimization  
- Structure editing or modification  
→ Route to **small-molecule-generation-optimization**

### Structure-Based Simulation
- Docking, pose prediction, molecular dynamics  
- Binding affinity or interaction simulation  
→ Route to appropriate docking/dynamics skill (not yet defined)

### Chemistry Execution
- Retrosynthesis, reaction planning, synthesis feasibility  

If detected, **reject this skill** and route accordingly.

## Allowed Tools (Exclusive List)

Only these tools may be invoked while this skill is active:

- `sim_screen`  
- `ph_screening`  
- `substructure_screen`  
- `novelty`  
- `tanimoto`

**Any other tool invocation = planner error.**

### Tool-Specific Hard Constraints

- `sim_screen` (Similarity-Based Library Screening)  
  Screens libraries against one or more reference molecules using fingerprint-based similarity; outputs ranked/filtered subsets; **reference-relative mode only**; **never** predicts properties, claims activity, or modifies structures  

- `ph_screening` (Pharmacophore-Based Screening)  
  Screens against predefined pharmacophore hypotheses (2D/3D feature matching); allows scaffold diversity under shared constraints; **never** generates pharmacophores, predicts affinity, or performs docking/pose refinement  

- `substructure_screen` (Structural Rule Filtering)  
  Applies explicit inclusion/exclusion rules via graph-based or exact substructure matching; produces pass/fail or annotated outputs; **never** estimates toxicity numerically, ranks by desirability, or modifies structures  

- `novelty` (Novelty & Redundancy Control)  
  Measures novelty relative to reference set; identifies duplicates/near-duplicates; supports diversity-aware pruning; **comparative only**; **never** generates molecules or explores chemical space  

- `tanimoto` (Pairwise Similarity Computation)  
  Computes numeric similarity between exactly two molecules; primitive utility only; **never** screens libraries, enforces thresholds, or decides candidate retention  

If required inputs are missing or constraints violated, **reject the skill**.

## Tool Selection Heuristics (Soft Guidance)

- Use `sim_screen` for bulk reduction against known reference molecules  
- Use `ph_screening` when hypothesis-driven interaction features are key  
- Use `substructure_screen` for strict compliance, safety, or rule enforcement  
- Use `novelty` after primary screening to eliminate redundancy  
- Use `tanimoto` **only** as a low-level similarity primitive (not for selection)  

## Planner Rejection Signals (Strong Indicators)

Treat these phrases as **very strong** reasons to reject and re-route:

- “predict ADMET”, “toxicity”, “solubility”, “permeability”  
- “optimize molecule”, “design new compound”, “scaffold hop”  
- “dock against protein”, “simulate binding”, “pose”  

If the conversation shifts toward property prediction, molecule generation/optimization, docking/simulation, or synthesis planning during execution, stop using this skill and recommend switching to the appropriate analytical, generative, or simulation skill.