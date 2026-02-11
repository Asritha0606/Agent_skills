# =========================
# Skill Metadata
# =========================

name: interaction_and_binding_design
domain: protein_engineering
type: analytical

description: >
  This skill governs all reasoning and operations related to the
  analysis, design, and evaluation of molecular interactions between
  proteins, peptides, antibodies, enzymes, and small-molecule ligands.
  It focuses on understanding how molecular entities recognize,
  bind, stabilize, or interface with one another.

  The skill operates at the interaction layer, emphasizing binding
  interfaces, stability, compatibility, and interaction strength.
  Its purpose is to assess or design interaction feasibility and
  quality, rather than generating sequences or structures in isolation.

# =========================
# Intent & Scope
# =========================

intent_scope:
  - analyzing protein–protein interactions (PPI)
  - evaluating protein–ligand or peptide–ligand binding
  - designing or refining molecular binders
  - predicting interaction stability or interface quality
  - mapping interaction interfaces or contact regions
  - validating interaction feasibility before optimization

out_of_scope:
  - de novo sequence or molecule generation
  - primary 3D structure prediction
  - molecular dynamics simulations
  - chemical synthesis or reaction planning
  - standalone property prediction without interaction context

# =========================
# Input / Output Contract
# =========================

input_types:
  - protein_structure
  - antibody_structure
  - peptide_sequence
  - ligand_smiles
  - protein_complex_structure
  - chain_or_interface_specification

output_types:
  - interaction_metrics
  - binding_affinity_estimates
  - interface_annotations
  - stability_indicators
  - interaction_analysis_reports

# =========================
# Skill Selection Rules
# =========================

selection_rules:

  select_this_skill_when:
    - the user asks about binding, interaction, or complex formation
    - the task involves evaluating interaction strength or stability
    - the workflow requires interface-level reasoning
    - the goal is binder design or interaction validation
    - interaction quality determines downstream decision-making

  do_not_select_when:
    - the task is purely sequence or molecule generation
    - the user requests only structure prediction
    - the focus is on ADMET or standalone property scoring
    - no interacting molecular entities are involved
    - chemical-only reasoning without biological interaction context

# =========================
# Reasoning Characteristics
# =========================

reasoning_mode:
  - interface-focused
  - comparative
  - evaluative
  - validation-oriented

assumptions:
  - interacting entities are biologically meaningful
  - input structures or sequences are sufficiently accurate
  - interaction predictions are approximations, not experimental truth
  - interaction quality can guide downstream optimization or filtering

failure_modes:
  - incorrect interface identification
  - overestimation or underestimation of binding strength
  - incompatibility between input representations
  - misinterpretation of predictive interaction metrics
  - cascading errors if upstream structures are flawed

# =========================
# Inter-skill Relationships
# =========================

upstream_skills:
  - sequence_generation_and_design
  - structure_modeling_and_prediction
  - molecular_generation_and_optimization

downstream_skills:
  - property_prediction_and_analysis
  - screening_and_filtering
  - optimization_and_lead_refinement

complementary_skills:
  - structure_modeling_and_prediction
  - property_prediction_and_analysis
  - screening_and_filtering

# =========================
# Operational Notes
# =========================

notes:
  - This skill serves as a critical validation and decision layer
    between generation and optimization stages.
  - Outputs should be treated as predictive signals, not experimental
    confirmations of binding or efficacy.
  - Tool selection within this skill must consider interaction context
    (protein–protein vs protein–ligand vs peptide-based interactions).
  - Overuse of interaction predictions without downstream validation
    may lead to false confidence in candidate quality.

# =========================
# Tools
# =========================
tools:

### EvoBind

This tool enables **targeted peptide binder design** against a protein target using sequence-level information and structure-aware optimization. Rather than generating peptides in isolation, EvoBind is explicitly focused on **peptide–protein interaction design**, producing peptide candidates that are optimized to bind specific interface regions on a target protein.

EvoBind operates by identifying interaction hotspots on the target sequence, exploring peptide sequence space through an iterative, deep-learning-guided optimization process, and predicting peptide–protein complexes at the structural level. It supports both **linear and cyclic peptide design**, optional incorporation of **evolutionary context** via multiple sequence alignments, and control over peptide length and interface focus.

This tool is intended for workflows where the goal is **binder discovery, interface-specific optimization, or affinity maturation**, rather than general peptide generation or standalone protein modeling. By directly producing predicted complex structures, EvoBind bridges the gap between sequence design and interaction-level reasoning.

Conceptually, EvoBind sits at the intersection of **sequence design and structural interaction modeling**. It should be selected when binding geometry, interface specificity, and interaction confidence are central to the task. Outputs from this tool—complex structures, interaction metrics, and designed sequences—are suitable for downstream ranking, docking refinement, experimental planning, or therapeutic peptide development.

Predicted binders should be treated as **computational candidates**, not experimentally validated interactions. Downstream validation and filtering are strongly recommended before high-stakes decision-making.

### Stability Analysis

This tool enables **quantitative stability and interaction energy analysis** of protein complexes using physics-informed energy modeling. Rather than predicting structures or generating binders, it focuses on **evaluating the energetic quality and robustness of existing protein–protein interactions** within a supplied structural complex.

The Stability Analysis tool operates on experimentally resolved or computationally predicted PDB structures and applies the FoldX energy model to compute interaction energies, stability scores, and shared interaction patterns between specified chains. By allowing comparison against a reference set of ideal interactions, it supports interpretation of whether observed interfaces are stabilizing, suboptimal, or energetically unfavorable.

This tool is intended for workflows where **structural complexes already exist** and the objective is to assess interaction strength, rank complex variants, or identify stabilizing versus destabilizing interface features. Typical use cases include post-design validation, interface optimization studies, energetic hotspot analysis, and prioritization of candidates for experimental follow-up.

Conceptually, Stability Analysis sits **downstream of structure generation or binder design** and upstream of experimental decision-making. It does not alter structures or propose new designs; instead, it provides **diagnostic insight** into how well a given complex is likely to hold together under energetic considerations.

Outputs from this tool should be interpreted as **model-based energetic estimates**, useful for comparative reasoning and filtering rather than definitive thermodynamic measurements. Downstream validation or complementary analyses are recommended for high-confidence conclusions.


### GraphDTA

This tool enables **data-driven prediction of ligand–protein binding affinity or activity** using graph-based deep learning models. Rather than relying on explicit structural docking or physics-based simulations, GraphDTA infers interaction strength directly from **ligand chemical graphs (SMILES)** and **protein sequence information**.

The GraphDTA tool is intended for workflows where **rapid, scalable affinity estimation** is required across large ligand libraries or multiple protein targets. By supporting both regression (e.g., IC50, Kd) and classification modes, it allows the agent to reason about interaction strength either quantitatively or categorically, depending on the modeling objective.

This capability is especially valuable in **early-stage screening and prioritization pipelines**, where evaluating thousands of ligand–target pairs via docking or molecular dynamics would be computationally prohibitive. GraphDTA provides a fast approximation of binding likelihood or potency, enabling efficient filtering before more expensive structure-based analyses are applied.

Conceptually, this tool sits **between molecular generation/screening and detailed structural validation**. It does not produce binding poses or mechanistic explanations; instead, it supplies **predictive signals** that guide decision-making about which ligand–protein pairs merit deeper investigation.

Outputs from GraphDTA should be treated as **model-derived predictions**, suitable for ranking, triage, and hypothesis generation rather than definitive measurements of binding affinity.
