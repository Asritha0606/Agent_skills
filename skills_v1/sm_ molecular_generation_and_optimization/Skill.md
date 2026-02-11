# =========================
# Skill Metadata
# =========================

name: molecular_generation_and_optimization
domain: small_molecule
type: generative

description: >
  This skill governs all reasoning and operations related to the
  generation, editing, and optimization of small molecules and ligands.
  It enables the agent to design novel chemical entities or improve
  existing ones by exploring chemical space using fragment-based,
  pharmacophore-driven, reinforcement learning, or combinatorial
  approaches.

  The skill is generative and iterative in nature. Its purpose is to
  produce chemically valid, diverse, and task-relevant small molecules
  that can serve as candidates for downstream screening, docking,
  property prediction, or experimental evaluation.

# =========================
# Intent & Scope
# =========================

intent_scope:
  - de novo generation of small molecules or ligands
  - optimization of existing molecules for desired objectives
  - fragment-based or scaffold-based molecular design
  - exploration of chemical space under defined constraints
  - iterative improvement of molecules using feedback signals
  - preparation of candidate libraries for downstream analysis

out_of_scope:
  - predicting biochemical or ADMET properties
  - virtual screening or filtering of large libraries
  - molecular docking or dynamics simulations
  - chemical retrosynthesis or reaction planning
  - experimental validation or synthesis execution

# =========================
# Input / Output Contract
# =========================

input_types:
  - smiles
  - molecular_fragments
  - pharmacophore_constraints
  - optimization_objectives
  - seed_molecules

output_types:
  - generated_molecules
  - optimized_molecules
  - molecular_libraries
  - smiles_collections
  - candidate_sets

# =========================
# Skill Selection Rules
# =========================

selection_rules:

  select_this_skill_when:
    - the user requests generation of new small molecules
    - molecular optimization or modification is the primary goal
    - chemical space exploration is required
    - starting molecules or fragments need improvement
    - outputs are intended for screening or downstream evaluation

  do_not_select_when:
    - the task is property prediction or ADMET analysis
    - the user requests docking or interaction simulation
    - the task involves filtering or ranking existing libraries only
    - chemical synthesis pathways are being planned
    - no molecule generation or modification is required

# =========================
# Reasoning Characteristics
# =========================

reasoning_mode:
  - generative
  - exploratory
  - optimization-driven

assumptions:
  - generated molecules must be chemically valid
  - optimization objectives may be multi-dimensional
  - outputs are candidates, not final drugs
  - downstream evaluation is required for validation

failure_modes:
  - generation of chemically invalid or unstable molecules
  - overfitting to narrow optimization objectives
  - loss of diversity during optimization
  - producing candidates unsuitable for downstream workflows

# =========================
# Inter-skill Relationships
# =========================

upstream_skills:
  - none

downstream_skills:
  - screening_and_filtering
  - property_prediction_and_modeling
  - docking_and_dynamics

complementary_skills:
  - screening_and_filtering
  - property_prediction_and_modeling

# =========================
# Operational Notes
# =========================

notes:
  - This skill acts as the entry point for small-molecule discovery
    workflows, generating candidate chemical matter for further study.
  - Generated molecules should be evaluated using downstream screening
    and property prediction tools before prioritization.
  - Tool selection within this skill must consider whether the task is
    de novo generation, constrained design, or iterative optimization.
  - Maintaining chemical diversity while optimizing objectives is
    critical to avoid premature convergence.

# =========================
# Tools
# =========================
tools:
### Structure-Based Molecular Generation

This tool enables **binding-pocket–conditioned de novo generation of small molecules**, where candidate ligands are designed explicitly to fit the geometric and physicochemical characteristics of a protein binding site. Rather than exploring chemical space in an unconstrained manner, it anchors molecular generation to a known structural context, significantly increasing the relevance of produced candidates for downstream binding evaluation.

The Structure-Based Molecular Generation tool is intended for workflows where **a target protein structure and binding pocket are already defined**, and the primary goal is to generate novel ligands tailored to that pocket. By leveraging shape embeddings derived from the pocket geometry, the tool proposes molecular cores that are spatially compatible with the binding site and then decodes them into chemically valid molecules.

A key feature of this tool is its ability to **integrate property-based filtering during generation**. Global, platform-provided property models or user-supplied custom models can be applied as constraints, ensuring that generated molecules not only fit the pocket but also satisfy developability or design criteria such as drug-likeness or physicochemical bounds.

Conceptually, this tool represents a **precision-driven entry point into ligand design**, sitting downstream of binding-site identification and upstream of docking, screening, or property analysis. Its outputs should be treated as **structure-aware ligand hypotheses** that are subsequently evaluated and refined through downstream workflows.


### Substructure Extraction (Rationale Generation)

This tool enables **extraction of chemically meaningful substructures (rationales)** from existing small molecules. Rather than generating new ligands directly, it decomposes molecules into interpretable fragments that capture core chemical motifs responsible for activity, drug-likeness, or other desired properties.

The Substructure Extraction tool is intended for workflows where **fragment-level understanding or reuse is required** before generative design. By identifying rationales within input molecules, the agent can isolate reusable chemical building blocks that serve as seeds for downstream tasks such as fragment-based generation, scaffold hopping, or targeted optimization.

A key feature of this tool is its ability to **apply property-based filtering during extraction**. Global, platform-provided models or user-defined custom models can be used to constrain which substructures are retained, ensuring that extracted fragments satisfy predefined physicochemical or design criteria.

Conceptually, this tool functions as a **bridge between analysis and generation** in small-molecule workflows. It transforms raw molecular libraries into curated fragment sets that guide and constrain subsequent generative processes, improving interpretability and design efficiency.

### Substructure Merging (Rationale Composition)

This tool enables **composition of larger, more complex chemical substructures by merging previously extracted rationales**. Rather than operating on full molecules, it works at the fragment level, combining chemically meaningful substructures into composite motifs that can serve as advanced building blocks for downstream ligand generation.

The Substructure Merging tool is intended for workflows where **fragment-based design is iterative**, and higher-order chemical motifs must be constructed from simpler rationales. By merging outputs from substructure extraction steps, the agent can progressively build richer chemical hypotheses while maintaining interpretability and control over molecular complexity.

A key capability of this tool is its support for **property-based filtering during merging**. Global platform models or user-defined custom models can be applied to ensure that merged rationales satisfy predefined physicochemical or design constraints. This allows the agent to balance increased structural complexity with developability or domain-specific requirements.

Conceptually, this tool acts as a **fragment composition engine**, bridging raw rationale extraction and full molecule generation. Its outputs are best used as structured seeds for generative models, scaffold construction, or targeted optimization workflows rather than as final drug-like molecules.

### Fragment-Based Molecular Generation (GFlowNet Inference)

This tool enables **de novo small-molecule generation using a fragment-based generative paradigm**, where molecules are assembled incrementally from learned fragment distributions rather than generated atom-by-atom in an unconstrained fashion. By operating at the fragment level, it balances chemical validity, diversity, and interpretability while efficiently exploring chemical space.

The GFlowNet-based generation tool is intended for workflows where **fragment-aware ligand discovery or optimization is required**, particularly when generation must be guided by property objectives. By leveraging pretrained or fine-tuned generative flow models, the agent can produce chemically coherent molecules while steering the generation process toward desirable regions of chemical space.

A key strength of this tool is its ability to **incorporate property-based constraints during generation**. Both platform-provided global models and user-defined custom models can be applied as filters, allowing the agent to bias generation toward molecules that satisfy drug-likeness, physicochemical, or project-specific criteria without post hoc filtering.

Conceptually, this tool represents a **controlled yet exploratory generation mechanism** within small-molecule design. It is especially useful when the agent needs to generate candidate libraries that are both diverse and aligned with predefined objectives, serving as a robust entry point for downstream screening, docking, or optimization workflows.

### Edit-Based Molecular Generation (MARS Inference)

This tool enables **iterative, edit-based generation of small molecules starting from a known reference scaffold**. Rather than generating molecules from scratch, it explores local chemical space by applying controlled modifications to an existing molecule, preserving core structural features while introducing targeted variations.

The Edit-Based Molecular Generation tool is intended for workflows where **lead optimization or analog exploration** is the primary objective. By operating on a reference SMILES, the agent can efficiently generate close analogs that maintain the parent molecule’s identity while improving specific properties such as drug-likeness, physicochemical balance, or project-defined objectives.

A key strength of this tool is its ability to **enforce property constraints during the editing process**. Global platform models and user-defined custom models can be applied to guide edits, ensuring that generated analogs remain within acceptable design boundaries rather than relying on post-generation filtering.

Conceptually, this tool represents a **local optimization engine** within small-molecule design workflows. It is best suited for refinement and diversification around validated scaffolds, serving as a natural complement to de novo or fragment-based generation methods.

### Pharmacophore-Driven Molecular Generation

This tool enables **generation of small molecules that explicitly satisfy a predefined pharmacophore hypothesis**. Rather than relying on unconstrained chemical exploration or local scaffold edits, it anchors molecular generation to a set of spatial and functional interaction requirements derived from prior knowledge or analysis.

The Pharmacophore-Driven Molecular Generation tool is intended for workflows where **key interaction features are already known**, such as hydrogen-bond donors or acceptors, hydrophobic centers, aromatic features, or spatial constraints inferred from ligands or binding sites. By conditioning generation on a pharmacophore hypothesis, the agent ensures that produced molecules are aligned with essential binding requirements from the outset.

This capability is especially valuable in **target-focused ligand discovery**, **hypothesis-driven design**, and **lead expansion workflows**, where preserving critical interaction patterns is more important than broad chemical diversity. Generated molecules inherently respect the pharmacophore constraints, reducing downstream filtering burden.

Conceptually, this tool represents a **knowledge-guided generation strategy**, sitting between fully structure-based generation and unconstrained de novo design. Its outputs should be treated as pharmacophore-consistent ligand candidates suitable for subsequent docking, screening, or optimization.



### R-Group Enumeration and Combinatorial Library Design

This tool enables **systematic combinatorial expansion of chemical scaffolds** by performing R-group enumeration using predefined fragment libraries. Rather than generating molecules through learned generative models, it follows a deterministic, chemistry-driven strategy where substituents are attached to explicit R-group positions on a scaffold.

The R-Group Enumeration tool is intended for workflows where **scaffold identity is fixed** and the goal is to explore substituent diversity in a controlled, interpretable manner. By combining scaffolds with curated building blocks, the agent can generate comprehensive combinatorial libraries that preserve core chemical features while systematically varying peripheral groups.

This approach is especially valuable in **structure–activity relationship (SAR) studies**, **lead expansion**, and **late-stage optimization**, where medicinal chemistry intuition guides which positions and fragments are explored. Optional property models can be applied during enumeration to evaluate or prioritize generated molecules, reducing the size of the library before downstream screening.

Conceptually, this tool represents a **rule-based, chemistry-first generation strategy**. It complements machine-learning–based generators by offering transparency, reproducibility, and precise control over molecular modifications, making it ideal for hypothesis-driven and synthesis-aware design workflows.
### Monte Carlo Tree Search–Based Lead Optimization

This tool performs **goal-directed optimization of a single lead molecule** using Monte Carlo Tree Search (MCTS). Rather than enumerating combinatorial libraries or applying unconstrained edits, it treats molecular optimization as a sequential decision-making problem, where each chemical modification is evaluated based on a reward function.

The MCTS-based optimization tool is intended for workflows where a **validated hit or lead molecule already exists**, and the objective is to improve its overall profile across multiple criteria such as ADMET properties, physicochemical balance, and target selectivity. By exploring possible molecular edits as branches in a search tree, the method efficiently balances exploration of new chemical modifications with exploitation of promising regions of chemical space.

A key strength of this approach is its ability to **integrate target and off-target information** alongside global and custom property constraints. This allows the agent to optimize molecules holistically, rather than focusing on a single metric. Because the method does not require model fine-tuning, it is well suited for rapid lead refinement and late-stage optimization workflows.

Conceptually, this tool represents a **reinforcement-learning–style optimization engine** for small molecules. It should be used when precise, multi-objective lead optimization is required, and when generation must be tightly guided by explicit scoring and constraint definitions.


### Reinforcement Learning–Based SMILES Augmentation for Lead Generation

This tool performs **reinforcement-learning–driven molecular generation using SMILES augmentation** to expand and optimize chemical space beyond standard generative approaches. By augmenting SMILES representations and applying reward-guided learning, the model explores diverse molecular variants while maintaining alignment with desired property objectives.

The Augmented SMILES Generation tool is intended for workflows where **lead molecules need to be discovered or improved through guided exploration**, especially when existing generative models may be overly conservative or trapped in narrow chemical regions. SMILES augmentation enables multiple syntactic representations of the same molecule, which increases exploration efficiency and robustness during reinforcement learning.

A defining characteristic of this approach is its ability to **optimize molecules directly against multi-objective reward functions**, including ADMET properties, developability metrics, and selectivity considerations. Optional target and off-target information can further guide optimization toward molecules with improved biological relevance.

Conceptually, this tool represents a **learning-driven exploration engine** for small-molecule design. It is best suited for early- to mid-stage lead discovery and optimization scenarios where broader chemical diversity is desired, and where reinforcement learning can iteratively improve candidate quality without strict dependence on a single starting scaffold.




### Reinforcement Learning–Based Lead Generation (REINVENT Framework)

This tool enables **reinforcement-learning–driven molecular generation using the REINVENT framework**, a widely adopted approach for property-optimized small-molecule design. It generates new molecules by learning a policy that maximizes a user-defined reward function, optionally biased by known hit molecules.

The REINVENT-based generation tool is intended for workflows where **property-guided lead discovery or improvement** is required, and where reinforcement learning can iteratively shape the chemical distribution toward desirable regions. By incorporating reward functions that encode physicochemical, ADMET, or selectivity objectives, the model can prioritize molecules that satisfy multiple constraints simultaneously.

Unlike local optimization methods, this approach performs **global exploration of chemical space**, making it suitable for early-stage lead generation or diversification around known chemical motifs. Optional fine-tuning using input SMILES allows the agent to bias generation toward project-relevant chemistry without fully constraining exploration.

Conceptually, this tool represents a **policy-based reinforcement learning engine** for molecular generation. It should be used when the objective is to generate diverse, property-optimized molecules guided by explicit reward definitions, rather than to perform deterministic enumeration or strictly local scaffold refinement.


### Fragment-Based Local Molecular Optimization (CREM)

This tool performs **local optimization of a small-molecule lead using fragment-level modifications**. Instead of generating molecules globally or exploring full chemical space, it applies chemically valid fragment insertions, deletions, or replacements to a reference molecule, producing close analogs that preserve the core identity of the starting compound.

The Fragment-Based Local Optimization tool is intended for workflows where a **lead molecule already exists**, and the objective is to explore nearby chemical space through interpretable, chemistry-driven edits. By operating at the fragment level, the tool mimics medicinal chemistry strategies such as bioisosteric replacement, fragment swapping, and localized scaffold refinement.

Unlike reinforcement-learning–based optimizers, this approach does not rely on reward policies or sequential decision trees. Instead, it systematically generates analogs through fragment operations and optionally evaluates them against ADMET or property constraints. This makes it especially suitable for **lead optimization, SAR exploration, and hypothesis-driven refinement** where transparency and chemical plausibility are critical.

Conceptually, this tool represents a **deterministic, fragment-aware local optimizer**. It should be used when controlled, interpretable modifications around a known lead are preferred over broader generative exploration or stochastic optimization methods.
