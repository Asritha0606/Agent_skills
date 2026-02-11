# =========================
# Skill Metadata
# =========================

name: property_prediction_and_analysis
domain: protein_engineering
type: analytical

description: >
  This skill governs all reasoning and operations related to the
  evaluation, prediction, and interpretation of biochemical,
  biophysical, functional, and immunological properties of biological
  macromolecules. It enables the agent to assess properties of proteins,
  peptides, antibodies, and enzymes based on sequence- or structure-level
  inputs.

  The skill is strictly evaluative in nature. Its purpose is not to
  generate or modify sequences or structures, but to analyze existing
  candidates and provide quantitative or qualitative insights that
  inform downstream decision-making, prioritization, or experimental
  planning.

# =========================
# Intent & Scope
# =========================

intent_scope:
  - predicting biophysical properties such as stability or solubility
  - evaluating functional or developability-related characteristics
  - assessing immunogenicity or antigenicity risks
  - analyzing sequence or structure-derived metrics
  - comparing and ranking candidates based on predicted properties
  - supporting decision-making in protein engineering workflows

out_of_scope:
  - generating or redesigning protein, antibody, or peptide sequences
  - predicting or generating three-dimensional structures
  - performing molecular docking or dynamics simulations
  - chemical synthesis or retrosynthesis planning
  - direct experimental validation or wet-lab execution

# =========================
# Input / Output Contract
# =========================

input_types:
  - protein_sequence
  - antibody_sequence
  - peptide_sequence
  - protein_structure
  - antibody_structure
  - candidate_set

output_types:
  - predicted_property
  - property_scores
  - comparative_analysis
  - ranked_candidates
  - risk_or_quality_assessment

# =========================
# Skill Selection Rules
# =========================

selection_rules:

  select_this_skill_when:
    - the user requests evaluation or prediction of molecular properties
    - the primary goal is analysis rather than generation or modeling
    - the task involves comparing, ranking, or filtering candidates
    - sequence or structural inputs are provided for assessment
    - the output is intended to guide downstream design or experimentation

  do_not_select_when:
    - the user asks to generate new sequences or structures
    - the task focuses on folding or backbone modeling
    - the request involves docking or interaction simulation
    - the task is purely chemical or ligand-centric

# =========================
# Reasoning Characteristics
# =========================

reasoning_mode:
  - evaluative
  - comparative
  - analytical

assumptions:
  - input sequences or structures are valid representations
  - predicted properties are approximations, not experimental truth
  - multiple properties may need to be considered jointly
  - outputs are used to inform, not replace, expert judgment

failure_modes:
  - over-reliance on predicted scores without context
  - misinterpretation of model-derived metrics as definitive
  - propagation of errors from poor-quality inputs
  - conflicting property signals across different analyses

# =========================
# Inter-skill Relationships
# =========================

upstream_skills:
  - sequence_generation_and_design
  - structure_modeling_and_prediction

downstream_skills:
  - none

complementary_skills:
  - sequence_generation_and_design
  - structure_modeling_and_prediction

# =========================
# Operational Notes
# =========================

notes:
  - This skill acts as a critical decision-support layer in protein
    engineering pipelines, enabling informed prioritization and risk
    assessment.
  - Outputs should be treated as predictive signals rather than
    experimentally validated measurements.
  - Tool selection within this skill must be driven by the specific
    property of interest and the nature of the provided inputs
    (sequence-based vs structure-based).
  - Combining multiple property analyses is often necessary to form
    robust conclusions.


# =========================
# Tools
# =========================
tools:
### Peptide Property Prediction

This tool enables **predictive evaluation of immunological and safety-related properties of peptide sequences**, providing insights that are critical for candidate screening and risk assessment. Rather than generating or modifying peptides, it analyzes existing sequences to estimate properties that influence biological compatibility and downstream usability.

The Peptide Property Prediction tool is intended for workflows where **peptide candidates must be assessed for antigenicity, allergenicity, toxicity, or related immunological characteristics**. By applying specialized predictive models to a set of input peptides, it allows the agent to identify potentially promising candidates while flagging sequences that may pose safety or efficacy concerns.

This capability is especially valuable in **vaccine design**, **epitope analysis**, and **early-stage peptide screening**, where rapid, model-based evaluation can significantly narrow the candidate space before experimental validation. The tool supports comparative reasoning by producing structured scores that enable ranking, filtering, and prioritization of peptide sequences.

Conceptually, this tool represents a **risk- and quality-assessment layer** within peptide-centric workflows. Its outputs should be interpreted as predictive signals that guide decision-making rather than definitive measures of biological behavior.


### Antibody Property Prediction

This tool enables **model-based evaluation of functional, biophysical, and structural properties of antibody sequences** using specialized deep-learning approaches. Rather than generating or modifying antibodies, it analyzes existing heavy- and light-chain sequences to infer properties that influence stability, solubility, binding behavior, and structural organization.

The Antibody Property Prediction tool is intended for workflows where **rapid, sequence-level assessment of antibody developability or functionality is required**. By leveraging antibody-specific representations and numbering schemes, it provides contextualized predictions that account for canonical antibody architecture, particularly within complementarity-determining regions (CDRs).

This capability is especially valuable in **antibody engineering**, **developability screening**, and **candidate prioritization**, where early insight into stability, solubility, or binding-related characteristics can significantly reduce experimental cost. The tool also supports CDR identification, enabling downstream reasoning about antigen interaction regions without requiring explicit structural modeling.

Conceptually, this tool serves as a **decision-support layer for antibody-centric workflows**, translating raw sequence information into actionable property signals. Outputs should be interpreted as predictive indicators that guide optimization and selection, not as substitutes for experimental validation.


### NetsolP (Protein Solubility & Usability Prediction)

This tool enables **predictive evaluation of protein solubility and expression usability**, providing critical insight into how likely a protein sequence is to be successfully expressed and handled in practical experimental systems, particularly *E. coli*. Rather than modifying sequences or modeling structures, it evaluates existing protein sequences to estimate their developability and experimental feasibility.

The NetsolP tool is intended for workflows where **early-stage assessment of protein expression risk** is required. By applying transformer-based protein language models, it produces model-derived solubility and usability scores that help the agent identify sequences that may suffer from aggregation, poor expression, or handling difficulties.

This capability is especially valuable in **protein engineering**, **enzyme development**, and **biotherapeutic candidate screening**, where poor solubility or usability can lead to costly downstream failures. In addition to predictive scores, the tool provides a rich set of biochemical descriptors, enabling deeper comparative analysis across candidates.

Conceptually, NetsolP serves as a **developability and manufacturability signal generator**. Its outputs should be used to guide prioritization, filtering, and risk mitigation strategies, rather than being treated as definitive experimental outcomes.


### Pep_Patch (Protein Surface Electrostatic Analysis)

This tool enables **analysis of electrostatic surface patches on protein structures**, providing insight into charged regions that are likely to participate in molecular recognition events. Rather than evaluating global properties such as stability or solubility, it focuses on localized surface features that influence binding, specificity, and interaction propensity.

The Pep_Patch tool is intended for workflows where **surface electrostatics play a critical role**, such as epitope mapping, antibody–antigen interaction analysis, or protein–protein interface characterization. By identifying contiguous charged patches and summarizing their properties, it allows the agent to reason about how surface charge distribution may affect molecular interactions.

This capability is especially valuable in **immunological analysis**, **binder engineering**, and **interaction hypothesis generation**, where charged surface regions often correlate with functional interfaces. The output supports comparative reasoning across patches, helping prioritize regions for further structural analysis, docking, or experimental validation.

Conceptually, Pep_Patch serves as a **structure-derived interaction signal extractor**, translating geometric surface information into interpretable electrostatic features. Its outputs should be used to guide hypotheses rather than as definitive indicators of binding behavior.

### Cat_Pred (Enzyme Catalytic Efficiency Prediction)

This tool enables **predictive estimation of catalytic efficiency for enzyme–substrate pairs**, focusing on the kinetic parameter *kcat*. Rather than modeling structures or simulating reactions, it evaluates how effectively a given enzyme sequence is likely to catalyze a specific substrate, based on learned patterns from enzyme–substrate data.

The Cat_Pred tool is intended for workflows where **functional performance of enzymes must be assessed quantitatively**, such as enzyme engineering, biocatalyst screening, or pathway optimization. By combining protein sequence information with substrate SMILES representations, it allows the agent to reason about sequence–substrate compatibility and catalytic potential without requiring explicit structural models.

A key aspect of this tool is the inclusion of **uncertainty estimates**, separating aleatoric and epistemic components. These uncertainty signals provide valuable context for decision-making, helping the agent distinguish between confident predictions and regions of limited model knowledge.

Conceptually, Cat_Pred serves as a **function-centric evaluation layer** for enzyme workflows. Its outputs should be used to guide prioritization, comparison, and hypothesis generation, rather than being interpreted as definitive kinetic measurements.



### Clean (Sequence Preprocessing & Standardization)

This tool enables **sanitization, validation, and standardization of raw protein sequences** prior to downstream analysis or modeling. Rather than predicting biological properties directly, it prepares sequence data to ensure correctness, consistency, and usability across subsequent tools and workflows.

The Clean tool is intended for workflows where **input sequence quality is uncertain or heterogeneous**, such as user-submitted datasets, aggregated sequence collections, or outputs from upstream generation tools. By removing invalid characters, normalizing formatting, and enforcing basic sequence validity checks, it reduces the risk of silent failures or misleading results in later stages.

In addition to basic cleaning, the tool can **annotate enzyme-related metadata**, such as predicting Enzyme Commission (EC) numbers and generating SMILES representations when applicable. This makes it especially useful as a preparatory step for enzyme-centric property prediction or functional analysis pipelines.

Conceptually, Clean serves as a **foundational preprocessing layer**, ensuring that all downstream property predictions operate on well-formed, standardized inputs. Its outputs should be treated as corrected representations of the original data, not as new biological inferences.





### Prodigy (Protein–Protein Binding Affinity Prediction)

This tool enables **structure-based prediction of protein–protein binding affinity**, focusing on quantitative estimation of interaction strength between chains in a molecular complex. Rather than modeling structures or simulating dynamics, it analyzes existing three-dimensional complexes to infer binding energetics from interfacial features.

The Prodigy tool is intended for workflows where **protein–protein interactions must be evaluated or compared**, such as binder assessment, complex stability analysis, or interface prioritization. By examining intermolecular contacts, surface properties, and interface composition, it provides affinity estimates that help the agent reason about which interactions are likely to be stronger or more stable.

This capability is especially valuable in **antibody–antigen analysis**, **protein complex evaluation**, and **binder engineering**, where structural data is available and interaction strength is a key decision factor. The tool supports both targeted analysis of specified chain pairs and broad evaluation of all chain–chain combinations within a complex.

Conceptually, Prodigy serves as a **structure-derived interaction scoring mechanism**, translating geometric and contact-level information into interpretable affinity signals. Its outputs should be treated as predictive estimates that guide comparison and prioritization, not as definitive thermodynamic measurements.
