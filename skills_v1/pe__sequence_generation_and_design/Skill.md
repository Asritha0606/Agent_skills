# =========================
# Skill Metadata
# =========================

name: sequence_generation_and_design
domain: protein_engineering
type: generative

description: >
  This skill governs all reasoning and operations related to the creation,
  redesign, and optimization of biological sequences at the primary sequence
  level. It enables the agent to generate proteins, antibodies, enzymes, and
  peptides either from scratch (de novo), by modifying existing sequences, or
  by conditioning sequence generation on functional or structural constraints.
  
  The skill is strictly concerned with sequence-level design decisions and
  deliberately excludes structural prediction, property evaluation, or
  interaction analysis. Its role is to propose candidate biological sequences
  that can later be validated, modeled, or optimized by downstream skills.

# =========================
# Intent & Scope
# =========================

intent_scope:
  - generating novel protein sequences for discovery or exploration
  - designing antibodies when no prior sequence exists
  - redesigning or optimizing existing protein or antibody sequences
  - generating enzyme sequences from functional or EC-number descriptions
  - generating or modifying peptide sequences for screening or binding studies
  - exploring sequence diversity in early-stage biological research
  - producing candidate sequences to be evaluated by downstream skills

out_of_scope:
  - predicting or modeling 3D structures from sequences
  - evaluating stability, solubility, antigenicity, or other properties
  - estimating binding affinity or molecular interactions
  - performing docking, dynamics, or simulation-based reasoning
  - planning chemical synthesis or reaction pathways
  - ranking or scoring sequences based on experimental fitness

# =========================
# Input / Output Contract
# =========================

input_types:
  - none
  - protein_sequence
  - antibody_sequence
  - peptide_sequence
  - protein_structure
  - antibody_backbone
  - functional_description

output_types:
  - protein_sequence
  - antibody_sequence
  - peptide_sequence

# =========================
# Skill Selection Rules
# =========================

selection_rules:

  select_this_skill_when:
    - the user explicitly asks to generate, design, redesign, or optimize a biological sequence
    - the primary artifact requested by the user is a protein, antibody, enzyme, or peptide sequence
    - the task involves sequence creation, mutation, or refinement rather than evaluation
    - the user provides sequence, structure, or functional context specifically to guide sequence design
    - the goal is to produce candidate sequences for downstream modeling or analysis

  do_not_select_when:
    - the user explicitly requests 3D structure prediction or modeling
    - the user asks for property prediction, scoring, or developability assessment
    - the task focuses on binding affinity, interaction analysis, or docking
    - the task involves chemical synthesis, retrosynthesis, or reaction prediction
    - the user intent is purely evaluative rather than generative

# =========================
# Reasoning Characteristics
# =========================

reasoning_mode:
  - exploratory
  - constrained
  - optimization

assumptions:
  - sequence-level design is sufficient to address the user’s immediate intent
  - generated sequences may require downstream validation or refinement
  - provided inputs (sequences, structures, or functional descriptions) are biologically meaningful
  - diversity and feasibility must be balanced based on the user’s stated or implied goals

failure_modes:
  - generation of sequences that are biologically implausible or non-functional
  - excessive randomness leading to low downstream utility
  - over-constrained design that limits meaningful exploration of sequence space
  - misalignment between user intent and conditioning inputs
  - producing sequences that cannot be reasonably validated downstream

# =========================
# Inter-skill Relationships
# =========================

upstream_skills:
  - none

downstream_skills:
  - structure_modeling_and_prediction
  - property_prediction_and_analysis

complementary_skills:
  - structure_modeling_and_prediction
  - property_prediction_and_analysis

# =========================
# Operational Notes
# =========================

notes:
  - This skill serves as the foundational entry point for all sequence-centric
    workflows in protein engineering.
  - It is intentionally agnostic to the execution details of individual tools;
    tool selection must be driven by input availability, conditioning constraints,
    and user intent within the bounds of this skill.
  - This skill does not judge the quality, stability, or binding performance of
    generated sequences; such judgments must be delegated to downstream skills.
  - Overuse of de novo generation when refinement or structure-conditioned design
    is appropriate should be avoided by enforcing strict tool constraints.




# =========================
# Tools
# =========================

tools:

### Random Antibody Generation

This tool enables **de novo exploration of antibody sequence space** by generating fully synthetic heavy- and light-chain antibody sequences without relying on any prior sequence, structural template, or germline constraint.

It is intended for **early-stage discovery workflows**, where the objective is to explore broad sequence diversity rather than refine or optimize an existing antibody. By producing novel antibody variants assembled without conditioning on known frameworks or structures, this tool supports tasks such as initializing large synthetic libraries, probing unexplored regions of antibody sequence space, and generating candidate sequences for downstream modeling or experimental screening.

The sequences produced by this tool are best viewed as **raw candidates**. They are not expected to be immediately optimal in terms of binding affinity, stability, or developability. Instead, they serve as starting points for subsequent refinement, structural modeling, or property evaluation using downstream skills.

This tool should be conceptually distinguished from tools that perform **targeted modification** or **structure-guided design**. It prioritizes diversity and novelty over precision and should therefore be used when exploration is more important than control.



### CDR Generator

This tool enables **focused engineering of antibody complementarity-determining regions (CDRs)** while preserving the surrounding framework architecture of the antibody. Rather than generating entire antibodies from scratch, it allows the agent to selectively redesign specific CDR loops—such as HCDR1, HCDR2, or HCDR3—to explore alternative binding motifs within an otherwise fixed antibody scaffold.

The CDR Generator is intended for **precision-driven antibody optimization workflows**, where an existing antibody sequence already provides a viable structural and functional foundation. By constraining sequence changes to designated CDR regions, the tool supports rational exploration of binding diversity while minimizing disruption to antibody stability, folding, and framework integrity.

This tool is especially relevant for **affinity maturation**, **specificity tuning**, and **epitope-focused redesign**, where small, localized sequence variations can yield meaningful improvements in antigen recognition. It should be conceptually distinguished from de novo generation tools, as its purpose is refinement and enhancement rather than broad discovery.

Outputs from this tool are best treated as **candidate variants** that require downstream validation through structure modeling, docking, or experimental assays to confirm improvements in binding performance or developability.


### Antibody Backbone to Sequence Generation

This tool enables **structure-guided antibody sequence design** by generating amino acid sequences that are explicitly constrained to fit a provided antibody backbone conformation. Unlike de novo generation or localized CDR mutagenesis, this approach treats the three-dimensional backbone as a fixed structural truth and designs sequences that are compatible with its geometry.

The tool is intended for workflows where **structural fidelity is paramount**—for example, when a reliable antibody structure is already available and sequence design must preserve the underlying fold. By conditioning sequence generation on the backbone, the agent can propose variants that are physically plausible, sterically compatible, and suitable for downstream structural refinement or validation.

This capability is especially relevant in **antibody humanization**, **stability engineering**, and **structure-aware affinity maturation**, where unconstrained sequence exploration would be inappropriate. Conceptually, this tool occupies the middle ground between broad discovery and fine-grained loop mutagenesis: it redesigns sequences globally, but always within the limits imposed by a known structure.

Outputs from this tool should be treated as **structure-consistent candidates** that can be further evaluated through docking, dynamics, or property prediction workflows.


### Function-Based Enzyme Generation

This tool enables **function-driven enzyme sequence design**, where the defining constraint is the catalytic activity the enzyme must perform rather than a specific sequence or structure. By conditioning sequence generation on an Enzyme Commission (EC) number, the tool allows the agent to explore enzyme sequence space that is aligned with a well-defined biochemical reaction class.

The Function-Based Enzyme Generation tool is intended for scenarios where **functional identity precedes structural or evolutionary specificity**. Instead of starting from an existing enzyme scaffold, it generates diverse enzyme candidates that are statistically consistent with known families performing the same reaction, while still allowing substantial sequence variation.

This capability is especially valuable for **biocatalyst discovery**, **enzyme engineering**, and **benchmarking studies**, where multiple candidate sequences sharing the same catalytic role are required. It supports exploratory workflows aimed at identifying promising enzyme variants before committing to structure modeling, mutagenesis, or experimental validation.

Conceptually, this tool sits between unconstrained de novo protein generation and structure-guided design. It narrows sequence space using functional knowledge (EC classification) while remaining agnostic to any particular fold or backbone, making it suitable for broad yet purposeful exploration of functional enzyme diversity.



### Sequence-Based Peptide Generation

This tool enables **peptide-centric sequence design** by generating short peptide sequences derived from, or related to, a provided antigen or parent protein sequence. Instead of operating at the level of full-length proteins or antibodies, it focuses on extracting and diversifying localized sequence regions that are likely to be biologically relevant, such as epitopes or binding motifs.

The Sequence-Based Peptide Generation tool is intended for workflows where **localized sequence fragments** are the primary object of interest. By sampling windows from an input sequence and applying controlled diversification, it supports exploration of peptide variants that retain contextual relevance to the original antigen while introducing sequence diversity suitable for screening or analysis.

This capability is especially valuable for **epitope discovery**, **immunogenic hotspot exploration**, and **peptide-based binding or screening studies**, where short, fixed-length sequences are required rather than full protein constructs. Conceptually, this tool bridges the gap between raw sequence analysis and downstream predictive or experimental workflows by producing peptide candidates that are both context-aware and diverse.

Outputs from this tool should be treated as **screening candidates**, not finalized designs. Further evaluation—such as binding prediction, immunogenicity assessment, or experimental validation—is typically required to determine their functional relevance.




### Random Controlled Mutagenesis

This tool enables **controlled exploration of protein sequence variants** by introducing random mutations into one or more existing sequences while explicitly preserving user-specified residues. Unlike de novo generation, it operates on known sequences and simulates evolutionary variation in a constrained and interpretable manner.

The Random Controlled Mutagenesis tool is designed for workflows where **sequence robustness, tolerance, or incremental improvement** is being investigated. By allowing the agent to protect critical residues—such as catalytic sites, binding motifs, or structurally important positions—it supports mutation-driven exploration without destroying known functional or structural features.

This capability is especially useful for **stability engineering**, **activity enhancement**, and **directed evolution–style simulations**, where controlled randomness can reveal beneficial variants while maintaining biological plausibility. Conceptually, this tool occupies the space between fine-grained targeted edits and unconstrained random generation: it enables broad variation, but within explicitly defined boundaries.

Outputs from this tool should be interpreted as **mutation libraries** rather than optimized designs. Downstream evaluation—such as property prediction, structure modeling, or experimental screening—is required to identify high-value variants.



### Unconditional Sampling

This tool enables **fully unconstrained de novo protein sequence generation**, producing entirely new protein sequences without conditioning on known function, structure, evolutionary lineage, or prior examples. It is designed for situations where the objective is to explore protein sequence space as broadly as possible, without imposing assumptions or design biases.

Unconditional Sampling is conceptually useful for **sequence space exploration**, **generative model benchmarking**, and **hypothesis-free discovery**, where diversity and novelty are prioritized over control or specificity. By removing functional and structural constraints, the tool allows the agent to probe regions of sequence space that may not be reachable through guided or refinement-based approaches.

The outputs from this tool are best understood as **raw, exploratory candidates**. While the generated sequences are biologically plausible, they are not optimized for any particular property, activity, or fold. As such, this tool is most effective when paired with downstream structure modeling, property prediction, or experimental screening to assess relevance and utility.

Conceptually, this tool represents the **maximum-diversity endpoint** within the sequence generation spectrum. It should be selected only when the user explicitly seeks unconstrained exploration rather than targeted design or optimization.

### Ligand MPNN

This tool enables **ligand-conditioned protein sequence design**, where the primary objective is to generate protein sequences that are optimized to bind a specific small-molecule ligand. Unlike unconstrained generation or function-only conditioning, Ligand MPNN explicitly incorporates the geometric and chemical context of a ligand-binding environment into the sequence design process.

The Ligand MPNN tool is intended for workflows in which **binding compatibility is the dominant constraint**. By conditioning sequence generation on a protein–ligand complex or backbone structure, the agent can propose amino acid sequences that stabilize the ligand-binding site while maintaining overall structural plausibility. This makes the tool especially valuable for **binding-site redesign**, **ligand-specific optimization**, and **structure-aware affinity engineering**.

Conceptually, this tool represents the most **highly constrained end** of the sequence generation spectrum. It trades broad sequence diversity for precision, ensuring that generated sequences are tailored to a known ligand context. As such, it should be selected only when a reliable ligand-binding structure is available and when the goal is explicit ligand compatibility rather than exploratory sequence generation.

Outputs from this tool are best treated as **binding-optimized candidates** that can be further evaluated through docking, molecular dynamics, or experimental assays to confirm binding strength and specificity.
