# =========================
# Skill Metadata
# =========================

name: docking_dynamics_and_synergy
domain: molecular_modeling
type: simulation_and_evaluation

description: >
  This skill governs all reasoning and operations related to
  simulating molecular interactions, estimating binding affinities,
  analyzing dynamic behavior, and evaluating synergistic effects
  between biological or chemical entities.

  The skill focuses on interaction-level and time-dependent
  behavior rather than static structure or sequence information.
  Its purpose is to assess how proteins, peptides, and small
  molecules interact under modeled physical conditions, enabling
  predictions related to binding strength, stability, conformational
  changes, and cooperative or antagonistic effects.

  Outputs from this skill are inherently predictive and model-based,
  serving as computational approximations to experimental assays
  rather than definitive measurements.

# =========================
# Intent & Scope
# =========================

intent_scope:
  - molecular docking and pose evaluation
  - binding affinity or interaction strength estimation
  - simulation of molecular dynamics or flexibility
  - stability assessment of complexes over time
  - synergy or interaction analysis between molecules
  - ranking or comparison of interaction scenarios

out_of_scope:
  - sequence generation or redesign
  - de novo molecule or peptide generation
  - static structure prediction or backbone modeling
  - chemical synthesis or retrosynthetic planning
  - experimental assay execution or wet-lab validation

# =========================
# Input / Output Contract
# =========================

input_types:
  - protein_structure
  - peptide_structure
  - ligand_structure
  - protein_ligand_complex
  - docking_pose_set
  - simulation_parameters

output_types:
  - docking_scores
  - binding_affinity_estimates
  - interaction_metrics
  - dynamic_trajectories
  - stability_indicators
  - synergy_scores

# =========================
# Skill Selection Rules
# =========================

selection_rules:

  select_this_skill_when:
    - the user requests docking or interaction simulation
    - binding strength or affinity estimation is required
    - dynamic stability or conformational behavior is queried
    - interaction comparison or ranking is the primary goal
    - evaluating cooperative or synergistic effects is requested

  do_not_select_when:
    - only structure prediction is requested
    - the task focuses on sequence or molecule generation
    - the user asks for property prediction without interaction context
    - chemical synthesis or reaction feasibility is the goal
    - static analysis without interaction modeling is sufficient

# =========================
# Reasoning Characteristics
# =========================

reasoning_mode:
  - probabilistic
  - comparative
  - simulation-driven

assumptions:
  - input structures are reasonable approximations
  - force fields and models introduce inherent uncertainty
  - predicted interactions reflect trends, not absolute truth
  - environmental parameters may strongly influence results

failure_modes:
  - inaccurate docking poses due to poor input structures
  - misleading affinity estimates from model limitations
  - instability artifacts in short or poorly parameterized simulations
  - overinterpretation of predictive scores as experimental evidence

# =========================
# Inter-skill Relationships
# =========================

upstream_skills:
  - structure_modeling_and_prediction
  - sequence_generation_and_design
  - molecular_generation_and_optimization

downstream_skills:
  - property_prediction_and_modeling
  - screening_and_filtering
  - clustering_and_analysis

complementary_skills:
  - interaction_and_binding_design
  - property_prediction_and_analysis

# =========================
# Operational Notes
# =========================

notes:
  - This skill represents the most compute-intensive stage of
    many molecular workflows and should be invoked selectively.
  - Outputs should be interpreted comparatively rather than
    as absolute quantitative truths.
  - Docking and dynamics results are sensitive to input quality
    and parameter selection.
  - When possible, interaction predictions should be followed
    by filtering, clustering, or validation steps to reduce noise.



# =========================
# Tools
# =========================

tools:

### DiffDock

This tool enables **deep-learning–based protein–ligand docking** by predicting plausible binding poses between a prepared protein structure and a library of small-molecule ligands. Rather than relying on classical scoring functions alone, DiffDock leverages learned geometric and chemical priors to infer binding orientations and rank candidate poses.

DiffDock is intended for workflows where **protein structures are already available and ligands are represented as SMILES**, and the primary objective is to identify likely binding modes and relative confidence rankings. It produces ranked ligand poses along with confidence estimates and interaction summaries, allowing downstream prioritization without exhaustive physics-based simulation.

This tool is particularly well-suited for **medium- to large-scale docking scenarios**, where rapid screening of ligand libraries against a fixed protein target is required. Optional pose file generation enables visual inspection or subsequent refinement using higher-fidelity methods.

Conceptually, DiffDock sits at the **intersection of structure-based screening and interaction evaluation**, providing a fast, model-driven approximation of binding behavior. Outputs should be treated as **predictive hypotheses**, suitable for ranking and filtering but not as definitive binding confirmations.


# =========================
# Tools
# =========================

tools:

### Drug–Drug Synergy (DDS)

This tool enables **computational prediction of drug–drug synergy** by estimating how two compounds—or a compound against a molecule library—interact across specific cancer or organ-derived cell-line categories. Rather than modeling physical binding or molecular interactions directly, it focuses on **phenotypic response patterns** learned from large-scale drug–response data.

The Drug–Drug Synergy tool is intended for workflows where the objective is to **prioritize combination therapies** or identify compounds that exhibit enhanced effectiveness when paired. It supports both pairwise drug evaluation and one-to-many screening scenarios, enabling efficient exploration of combinatorial therapeutic space.

Predictions are reported across multiple relevant cell lines and summarized as an average synergy score, providing a high-level quantitative signal for comparative ranking. These outputs are especially useful in **early-stage combination screening**, hypothesis generation, and experimental prioritization.

Conceptually, this tool operates at the **system-level interaction layer**, complementing molecular docking or binding analyses. Its predictions should be interpreted as **statistical synergy estimates**, not mechanistic explanations, and should be followed by experimental validation when used for decision-making.


### Molecular Dynamics Simulation (MDS)

This tool enables **physics-based molecular dynamics (MD) simulations** of protein–ligand complexes using established force-field and solvent models. Its purpose is to move beyond static structural predictions and capture the **time-evolution, stability, and conformational behavior** of biomolecular systems under defined thermodynamic conditions.

The Molecular Dynamics Simulation tool is intended for workflows where **dynamic stability, relaxation, or interaction persistence** must be evaluated after docking or structure prediction. By simulating atomic motions over time, it provides insight into binding stability, conformational drift, and system equilibration that cannot be inferred from single-structure models.

Users can specify environmental and simulation parameters such as temperature, pressure, solvent model, force fields, ionic conditions, and simulation duration. The tool outputs an equilibrated final structure and, optionally, a trajectory file that records molecular motion across simulation frames.

Conceptually, this tool represents the **highest-fidelity and highest-cost analysis layer** in structural workflows. It should be selected only when dynamic behavior is essential for decision-making, and its outputs should be interpreted in conjunction with prior docking or binding analyses rather than in isolation.

### GROMACS Molecular Dynamics

This tool enables **high-performance, physics-based molecular dynamics simulations** using the GROMACS engine. It is designed for executing full-scale MD simulations of protein–ligand systems with fine-grained control over simulation parameters, force fields, solvent models, and trajectory sampling frequency.

The GROMACS Molecular Dynamics tool is intended for workflows that require **production-grade molecular simulations**, such as detailed stability assessment, long-timescale conformational analysis, or rigorous post-docking validation. Compared to lightweight or abstraction-layer MD tools, this capability provides greater configurability and computational rigor, making it suitable for advanced structural studies.

Users explicitly define simulation duration, temperature, pressure, force-field family, solvent model, and snapshot intervals. The tool outputs both an equilibrated final structure and a time-resolved trajectory, enabling downstream analysis of structural drift, interaction persistence, and conformational ensembles.

Conceptually, this tool represents a **low-level, high-control molecular simulation backend**. It should be selected when simulation fidelity and configurability are prioritized over speed or cost, and when the workflow explicitly demands GROMACS-based execution rather than abstracted MD engines.
