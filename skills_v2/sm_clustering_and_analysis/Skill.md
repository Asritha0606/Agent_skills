---
name: small-molecule-clustering-analysis
description: Organizes and analyzes existing small-molecule libraries through clustering, grouping, scaffold identification, diversity assessment, redundancy detection, and normalization — producing descriptive abstractions (clusters, representatives, similarity matrices, chemical series) without ranking, selecting, filtering, predicting properties, generating, modifying, or simulating molecules. Use only when the user explicitly requests clustering molecules, analyzing diversity/redundancy, identifying scaffolds/series, selecting representatives (not winners), or organizing datasets for interpretability — and the task is purely descriptive/non-decisional.
metadata:
  domain: Small Molecule
  category: Analytical – Organization & Structure Discovery
  type: Non-decision Analytical
  maturity: Stable
  owner: ChemAI Platform Team
  original-id: sm__clustering_and_analysis
  allowed-tools: 
    - butina 
    - kmeans 
    - is_smiles 
    - is_multiple_smiles 
    - largest_mol 
    - is_cas 
    - query2smiles
---

# Small Molecule Clustering and Analysis

This skill **strictly imposes descriptive structure** on collections of existing small molecules. It groups, clusters, normalizes, and interprets libraries based on structural similarity, scaffold relationships, or learned representations — revealing latent organization, chemical series, diversity, and redundancy.

**Core purpose**  
- Reveal how molecules are related in chemical space  
- Identify scaffolds, chemical series, or representative structures  
- Reduce complexity for interpretability and downstream reasoning  
- Support diversity analysis and SAR understanding  
- Validate/organize datasets without decision-making  

**Strict boundaries**: All outputs are **descriptive organizational abstractions** (clusters, representatives, matrices), never prescriptive. No generation, modification, property prediction, ranking, selection, filtering, docking, molecular dynamics, or biological/quality claims.

## When to Activate This Skill (Mandatory Conditions)

Activate **only** if **all** conditions are true:

- Multiple molecules are provided (library or collection)  
- Task is purely organizational / descriptive (grouping, clustering, normalization, diversity analysis)  
- Goal is to understand structure/relationships — not to decide, rank, filter, or improve  
- No modification, prediction, simulation, or selection is requested  

**Typical activation signals**  
- “cluster these molecules”  
- “analyze diversity”  
- “group by scaffold”  
- “identify chemical series”  
- “organize this library”  
- “find representatives”

## Hard Guardrails – Do NOT Activate (Reject Immediately)

**Reject this skill entirely** (planner error) if **any** of these appear:

### Selection or Pruning
- Filtering, ranking, prioritizing candidates  
- Enforcing thresholds or rules for retention  
→ Route to **small-molecule-screening-filtering**

### Evaluation or Scoring
- ADMET, toxicity, drug-likeness, property prediction  
- Any form of scoring or classification  
→ Route to **small-molecule-property-prediction-modeling**

### Creation or Improvement
- Molecule generation, scaffold hopping, optimization  
- Structure editing or modification  
→ Route to **small-molecule-generation-optimization**

### Physics-Based Reasoning
- Docking, molecular dynamics, binding simulation  

If detected, **reject this skill** and route accordingly.

## Allowed Tools (Exclusive List)

Only these tools may be invoked while this skill is active:

- `butina`  
- `kmeans`  
- `is_smiles`  
- `is_multiple_smiles`  
- `largest_mol`  
- `is_cas`  
- `query2smiles`

**Any other tool invocation = planner error.**

### Tool-Specific Hard Constraints

- `butina` (Similarity & Scaffold Clustering)  
  Performs similarity-threshold or Murcko scaffold-based clustering; produces variable-size clusters and representatives; **organizational only**; **never** ranks clusters, implies quality/priority, or filters molecules  

- `kmeans` (Fixed-Partition Clustering)  
  Partitions into exactly K clusters using feature representations; enforces full assignment; **never** implies chemical relevance/optimality or used for selection/pruning  

- `is_smiles` (SMILES Syntax Validation)  
  Verifies syntactic correctness only; input validation gate; **never** performs chemical reasoning, normalization, or inference  

- `is_multiple_smiles` (Multi-Fragment Detection)  
  Detects disconnected components (salts, mixtures, counter-ions); flags issues; **never** selects fragments or discards components  

- `largest_mol` (Primary Fragment Extraction)  
  Extracts largest connected component for normalization only; **never** interpreted as biological curation or used for filtering/prioritization  

- `is_cas` (CAS Format Validation)  
  Validates CAS Registry Number syntax only; identifier classification; **never** resolves structures or confirms existence  

- `query2smiles` (Identifier Resolution)  
  Converts names, CAS, InChI, formulas → SMILES; enables structural workflows from text; **never** evaluates quality, predicts properties, or generates alternatives  

If required inputs are missing or constraints violated, **reject the skill**.

## Tool Selection Heuristics (Soft Guidance)

- Prefer `butina` for similarity- or scaffold-driven clustering and organization  
- Use `kmeans` **only** when a fixed number of clusters is explicitly requested  
- Apply `is_smiles` and `is_cas` early for input validation  
- Use `is_multiple_smiles` and `largest_mol` **strictly** for normalization  
- Use `query2smiles` **only** to resolve identifiers into SMILES  

## Planner Rejection Signals (Strong Indicators)

Treat these phrases as **very strong** reasons to reject and re-route:

- “filter”, “rank”, “select best”, “prioritize”  
- “predict”, “ADMET”, “toxicity”, “score”  
- “optimize”, “generate”, “design”  
- “dock”, “simulate”, “binding”  

If the conversation shifts toward selection, property prediction, generation/optimization, or simulation during execution, stop using this skill and recommend switching to the appropriate screening, property, generative, or simulation skill.