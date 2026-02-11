# =========================
# Skill Metadata
# =========================

name: structure_modeling_and_prediction
domain: protein_engineering
type: predictive

description: >
  This skill governs all reasoning and operations related to the
  generation, prediction, and interpretation of three-dimensional
  protein structures. It enables the agent to model protein backbones,
  full-length protein structures, antibody structures, and protein
  complexes from sequence or partial structural information.

  The skill is strictly concerned with structure-level representation
  and geometry. Its purpose is to translate sequence-level information
  into spatial models that capture folding, topology, and interaction
  potential, which can then be used for downstream analysis, design,
  or validation tasks.

# =========================
# Intent & Scope
# =========================

intent_scope:
  - predicting 3D structures from protein or antibody sequences
  - generating backbone or full structural models
  - modeling protein or antibody conformations
  - producing structural inputs for docking or dynamics
  - preparing structure files for downstream analysis or design

out_of_scope:
  - generating or redesigning protein sequences
  - evaluating binding affinity or interaction strength
  - predicting biophysical or functional properties
  - performing molecular docking or dynamics simulations
  - chemical synthesis or reaction planning

# =========================
# Input / Output Contract
# =========================

input_types:
  - protein_sequence
  - antibody_sequence
  - partial_structure
  - backbone_structure
  - protein_complex_structure

output_types:
  - protein_structure
  - antibody_structure
  - backbone_structure
  - protein_complex_structure

# =========================
# Skill Selection Rules
# =========================

selection_rules:

  select_this_skill_when:
    - the user explicitly requests 3D structure prediction or modeling
    - the primary artifact requested is a protein or antibody structure
    - the task involves folding, backbone generation, or spatial modeling
    - the output is intended for docking, dynamics, or structural analysis

  do_not_select_when:
    - the user asks for sequence generation or redesign
    - the task focuses on property prediction or scoring
    - the user requests binding affinity estimation or interaction analysis
    - the task involves chemical or ligand-based reasoning only

# =========================
# Reasoning Characteristics
# =========================

reasoning_mode:
  - constrained
  - comparative
  - evaluative

assumptions:
  - input sequences are valid biological sequences
  - predicted structures are approximations, not experimental truth
  - downstream tasks may require further refinement or validation
  - structural context is sufficient to support later analysis steps

failure_modes:
  - incorrect folding or topology prediction
  - ambiguity in flexible or disordered regions
  - propagation of sequence errors into structural artifacts
  - misuse of predicted structures as experimentally validated models

# =========================
# Inter-skill Relationships
# =========================

upstream_skills:
  - sequence_generation_and_design

downstream_skills:
  - property_prediction_and_analysis
  - docking_dynamics_and_synergy

complementary_skills:
  - sequence_generation_and_design
  - property_prediction_and_analysis

# =========================
# Operational Notes
# =========================

notes:
  - This skill acts as the structural backbone of protein engineering
    workflows, translating linear sequence information into spatial form.
  - Structural outputs should be treated as predictive models and not
    as substitutes for experimentally resolved structures.
  - Tool selection within this skill must account for input availability
    (sequence-only vs partial structure vs backbone) and desired fidelity.
  - Over-reliance on predicted structures without downstream validation
    can lead to misleading conclusions in later stages.



# =========================
# Tools
# =========================
tools:

### Backbone Generation

This tool enables **framework-level three-dimensional structure generation** for antibody heavy or light chains by producing realistic backbone conformations without assigning specific amino acid sequences. Rather than modeling full atomic detail or sequence-dependent features, it focuses on generating structurally plausible scaffolds that capture the canonical architecture of antibody frameworks.

The Backbone Generation tool is intended for workflows where **structural templates are required before sequence-level decisions are made**. By providing backbone-only models, it allows the agent to decouple structural reasoning from sequence design, supporting downstream tasks such as CDR grafting, structure-conditioned sequence generation, docking preparation, or stability analysis.

This capability is especially useful in **structure-first design pipelines**, where obtaining a reliable scaffold is a prerequisite for controlled sequence optimization. Conceptually, this tool sits upstream of structure-conditioned sequence design and downstream of purely sequence-centric workflows, serving as a bridge between abstract sequence intent and concrete three-dimensional geometry.

Outputs from this tool should be treated as **structural templates**, not finalized models. Further refinement, sequence assignment, or validation may be required depending on downstream objectives.
### Boltz Structure Prediction

This tool enables **general-purpose three-dimensional structure prediction** by transforming biological representations—such as protein sequences, nucleic acid sequences, or small-molecule descriptions—into spatial structural models. Its primary role is to bridge the gap between symbolic biological inputs and geometric representations that can be used for visualization, analysis, or downstream modeling.

The Boltz Structure Prediction tool is intended for workflows where **structural insight is required from sequence- or SMILES-level information**, but no prior structural template is available. By supporting multiple biomolecule types in a single invocation, it allows the agent to generate structural models for heterogeneous biological systems, such as proteins paired with ligands or nucleic acids.

This capability is especially valuable for **preparatory and exploratory stages** of structure-based workflows, where predicted models serve as inputs for docking, molecular dynamics, comparative analysis, or hypothesis generation. The resulting structures should be treated as **predictive approximations**, suitable for guiding reasoning but not as substitutes for experimentally resolved structures.

Conceptually, this tool represents a **broad-spectrum structure inference mechanism**. It should be selected when the task requires converting biological sequences or molecular encodings into three-dimensional form, rather than refining existing structures or generating new sequences.



### Monomer Structure Prediction

This tool enables **sequence-to-structure prediction for monomeric proteins**, transforming one or more amino acid sequences into three-dimensional structural models. It is specifically focused on single-chain (monomeric) proteins and does not attempt to model multimeric assemblies or complexes.

The Monomer Structure Prediction tool is intended for workflows where **primary sequence information is available and structural insight is required** without relying on existing templates or experimental structures. By operating in batch mode, it supports efficient prediction of multiple protein structures in a single run, making it suitable for exploratory analysis, large-scale annotation, or preparatory steps in structure-based pipelines.

A key aspect of this tool is the inclusion of **confidence metrics** alongside predicted structures. These metrics provide the agent with an explicit signal about the reliability of the generated models, enabling informed decisions about downstream usage such as docking, refinement, or experimental prioritization.

Conceptually, this tool represents a **focused and high-confidence pathway for monomeric protein folding inference**. It should be selected when the task is strictly sequence-based structure prediction for individual proteins, rather than backbone generation, complex modeling, or structure refinement.




### Motif Scaffolding

This tool enables **motif-driven protein structure design**, where a predefined structural or functional motif is preserved while a new surrounding scaffold is generated. Rather than predicting structures from sequence alone, it anchors the design process around a fixed motif whose geometry and spatial relationships must remain intact.

Motif Scaffolding is intended for workflows in which **specific residues or structural fragments carry critical functional meaning**, such as catalytic motifs, binding epitopes, or interaction hotspots. By embedding these motifs into newly generated scaffolds, the tool allows the agent to explore novel protein architectures that retain essential functionality while varying the surrounding structural context.

This capability is especially valuable in **immunogen design**, **enzyme stabilization**, and **binder engineering**, where maintaining the precise arrangement of key residues is non-negotiable. Conceptually, the tool occupies a space between pure structure prediction and unconstrained scaffold generation: it enforces strict local constraints while allowing global structural creativity.

Outputs from this tool should be treated as **motif-preserving structural hypotheses**. Further refinement, sequence assignment, or functional validation is typically required before downstream application.


### fpocket (Binding Site Detection)

This tool enables **structure-based identification and analysis of binding pockets** within a protein’s three-dimensional structure. Rather than predicting overall folds or generating new structures, it focuses on locating cavities and surface features that are potentially capable of accommodating small molecules or ligands.

The fpocket tool is intended for workflows where **binding-site discovery and characterization** are required after a protein structure has already been obtained. By analyzing geometric and physicochemical properties of cavities, it allows the agent to identify regions that are likely to be druggable, rank them by suitability, and extract structural representations of individual pockets.

This capability is especially valuable in **early-stage drug discovery**, **target assessment**, and **structure-based design pipelines**, where understanding where and how ligands might bind is a prerequisite for docking, virtual screening, or ligand optimization. Conceptually, fpocket operates downstream of structure prediction and upstream of docking or ligand-focused analysis.

Outputs from this tool should be treated as **binding-site hypotheses**. While they provide strong geometric and physicochemical signals, further validation—such as docking, dynamics, or experimental assays—is typically required to confirm functional relevance.
