---
name: small-molecule-property-prediction-modeling
description: Predicts physicochemical, ADMET, toxicity, drug-likeness, developability, and custom properties of existing small molecules from fixed structures (SMILES or datasets). Use only when the user explicitly requests property estimation, scoring, classification, ranking, filtering, annotation, uncertainty quantification, or custom model application — and molecules are already defined with no structural modification, generation, optimization, docking, molecular dynamics, retrosynthesis, or experimental/biological claims allowed.
metadata:
  domain: Small Molecule
  category: Analytical – Evaluation & Modeling
  maturity: Stable
  owner: ChemAI Platform Team
  original-id: sm__property_prediction_and_modeling
  allowed-tools: 
    - propinf 
    - automl 
    - phcore_gen
---

# Small Molecule Property Prediction and Modeling

This skill **strictly evaluates and models properties** of already-defined small molecules. It transforms fixed molecular structures (SMILES, datasets) into model-derived estimates of physicochemical, ADMET, toxicity, safety, drug-likeness, developability, or custom properties.

**Core purpose**  
- Predict ADMET and physicochemical properties  
- Estimate toxicity and safety risks  
- Assess drug-likeness and developability metrics  
- Apply custom or project-specific predictive models  
- Rank, filter, annotate, or triage molecule sets  
- Quantify prediction uncertainty and applicability domain  

**Strict boundaries**: Purely analytical and non-generative. All outputs are **model-derived estimates**, never experimental measurements, validated biological claims, or modified structures. No molecule creation, editing, optimization, docking, molecular dynamics, free-energy methods, retrosynthesis, reaction planning, or in vivo/in vitro assertions.

## When to Activate This Skill (All Conditions Mandatory)

Activate **only** if **every** condition is true:

- Molecules are already provided (SMILES, structures, or datasets)  
- Task requires property prediction, scoring, classification, ranking, filtering, annotation, or custom model application  
- No structural change, generation, or optimization is requested  
- Output goal is to inform decisions, guide downstream optimization, or triage candidates  

**Typical activation signals**  
- “predict ADMET / toxicity / solubility / permeability”  
- “rank compounds by property X”  
- “filter library above/below threshold”  
- “apply custom property model”  
- “estimate uncertainty / applicability domain”

## Hard Guardrails – Do NOT Activate (Reject Immediately)

**Reject this skill entirely** (planner error) if **any** of these appear:

### Molecular Creation or Modification
- De novo generation, scaffold hopping, R-group design  
- Lead optimization, structure editing  
- Keywords: “optimize structure”, “design new molecules”, “scaffold hop”

### Structure-Based Simulation
- Molecular docking, pose prediction  
- Molecular dynamics, free-energy calculations  
- Interaction scoring via 3D simulation  
- Keywords: “dock against protein”, “simulate binding”, “pose”

### Chemistry Execution
- Retrosynthesis, reaction planning  
- Synthesis feasibility or protocols  
- Keywords: “how to synthesize”, “reaction”, “retrosynthesis”

### Experimental or Biological Claims
- Asserting in vivo/in vitro efficacy or performance  

If the task involves **generation**, **optimization**, **docking/simulation**, or **synthesis planning**, **route to the appropriate skill** instead.

## Allowed Tools (Exclusive List)

Only these tools may be invoked while this skill is active:

- `propinf`  
- `automl`  
- `phcore_gen`

**Any other tool invocation = planner error.**

### Tool-Specific Hard Constraints

- `propinf` (Molecular Property Inference)  
  Accepts fixed molecular inputs (SMILES); predicts physicochemical, ADMET, toxicity, or custom properties; may output numeric values, categories, uncertainty; used for ranking/filtering/annotation; **never** modifies structures, performs docking/pose estimation, or claims experimental validation  

- `automl` (Custom Property Model Training)  
  Requires labeled molecular datasets; trains regression/classification models with chemically informed splitting; produces model artifacts for downstream inference; **never** generates/optimizes molecules, used without training data, or claims biological/clinical validation  

- `phcore_gen` (Pharmacophore Hypothesis Generation)  
  Extracts abstract interaction features (HBD/HBA, aromatic, hydrophobic, charged) from ligands or protein–ligand complexes; outputs representational hypotheses for interpretability or constraint setting; **never** generates molecules, scores affinity, performs docking, or structural optimization  

If required inputs are missing or constraints violated, **reject the skill**.

## Tool Selection Heuristics (Soft Guidance)

- Prefer `propinf` for direct, fast property inference on fixed molecules  
- Use `automl` **only** when labeled training data is explicitly provided and a reusable model is needed  
- Use `phcore_gen` **only** when interpretability or feature-level interaction constraints are requested  
- Avoid conflating numeric property scores with abstract pharmacophore hypotheses unless distinction is clear  

## Planner Rejection Signals (Strong Indicators)

Treat these phrases as **very strong** reasons to reject and re-route:

- “optimize structure”, “design new molecules”, “scaffold hop”  
- “dock against protein”, “simulate binding”, “pose”, “MD”  
- “how to synthesize”, “reaction”, “retrosynthesis”, “route”  

If the conversation shifts toward molecule generation, structural optimization, docking, simulation, or synthesis during execution, stop using this skill and recommend switching to the appropriate generative, simulation, or chemistry-planning skill.