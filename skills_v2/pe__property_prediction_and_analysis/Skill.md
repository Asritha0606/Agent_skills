---
name: protein-property-prediction-analysis
description: Predicts and analyzes biochemical, biophysical, functional, immunological, and interaction properties of existing proteins, peptides, antibodies, and enzymes from sequence or structure inputs. Use when the user explicitly requests property evaluation, developability risk assessment, candidate comparison/ranking, or prioritization support in protein engineering — and only when inputs are already-defined sequences or structures (no generation, mutation, design, structure prediction, docking, or simulation allowed).
metadata:
  domain: Protein Engineering
  category: Analytical – Property Evaluation
  maturity: Stable
  owner: BioAI Platform Team
  original-id: pe__property_prediction_and_analysis
  allowed-tools: 
    - peptide_property_prediction 
    - Antibody Property Prediction
    - NetsolP 
    - Pep_Patch 
    - Cat_Pred 
    - Prodigy 
    - Clean
---

# Protein Property Prediction and Analysis

This skill performs **strictly analytical** evaluation of already-defined biological macromolecules (proteins, peptides, antibodies, enzymes). It consumes sequence-level or structure-level inputs and produces quantitative or qualitative property predictions to support candidate assessment, risk analysis, and prioritization.

**Core purpose**  
Estimate developability risks, assess functional/interaction potential, compare and rank candidates, and provide decision-support signals.  
**No new biological entities are ever created.**

## When to Activate This Skill (Mandatory Conditions)

Activate **only** if **all** of these are true:

- User explicitly asks for property prediction, evaluation, analysis, comparison, ranking, or filtering of candidates
- Task is purely analytical / evaluative (not generative or transformative)
- Input consists of **existing** sequences, structures, or resolved complexes
- Goal is to assess risk, quality, performance, interaction strength, or guide downstream decisions

## Hard Guardrails – Do NOT Activate (Reject Immediately)

**Reject this skill entirely** (planner error) if the query contains **any** of:

### Generative / Transformative Intent
- Sequence generation, redesign, mutation
- Optimization, enhancement, improvement
- Keywords: “design”, “engineer”, “optimize”, “enhance”, “increase”, “improve”, “create”, “generate”

### Structural Modeling or Simulation
- Structure prediction, refinement
- Docking, molecular dynamics, binding pose generation
- Simulation of interactions

### Clinical / Therapeutic Reasoning
- Disease-specific claims
- Therapeutic efficacy, safety, or clinical outcome statements
- Phrases: “treat”, “cure”, “therapeutic outcome”

### Missing or Invalid Inputs for Tools
- Tools requiring structures used without user-provided structures
- Tools requiring complexes used without resolved complexes
- Enzyme catalysis prediction without substrate SMILES

If any upstream workflow requires **Sequence Generation & Design**, **Structure Modeling & Prediction**, or **Docking & Dynamics Simulation**, **route there instead**.

## Allowed Tools (Exclusive List)

Only these tools may be called while this skill is active:

- `peptide_property_prediction`
- `Antibody Property Prediction`
- `NetsolP`
- `Pep_Patch`
- `Cat_Pred`
- `Prodigy`
- `Clean`

**Invocation of any other tool = planner error.**

### Tool-Specific Hard Constraints

- `Clean`  
  Only for input validation and normalization. **Never** used to infer biological properties.

- `Pep_Patch`  
  Requires an **existing, user-provided** protein structure. **Never** triggers structure prediction.

- `Prodigy`  
  Requires a **resolved** protein–protein complex. Docking or pose generation invalidates skill use.

- `Cat_Pred`  
  Requires enzyme sequence **and** substrate SMILES. Strictly predictive — never for enzyme discovery.

If required inputs for a needed tool are missing, **reject the skill**.

## Tool Selection Heuristics (Soft Guidance)

- Prefer sequence-based tools (`peptide_property_prediction`, `Antibody Property Prediction`, `NetsolP`, `Cat_Pred`) when no structure is provided
- Use structure-based tools (`Pep_Patch`, `Prodigy`) **only** when structures/complexes are explicitly supplied
- Apply `Clean` early if input format or integrity is uncertain
- Avoid conflating sequence-derived and structure-derived conclusions without clear justification

## Planner Rejection Signals (Strong Indicators)

Treat these phrases as **very strong** reasons to reject this skill and re-route:

- “design”, “engineer”, “mutate”, “optimize”, “enhance”
- “dock”, “simulate”, “MD”, “molecular dynamics”
- “treat”, “cure”, “efficacy”, “safety profile”, “clinical”

If the task scope expands beyond pure property analysis during execution, the agent should stop using this skill and suggest switching to an appropriate generative or modeling skill.