# =========================
# Skill Metadata
# =========================

name: clustering_and_analysis
domain: small_molecule_design
type: analytical

description: >
  This skill governs all reasoning and operations related to the
  grouping, organization, and analytical comparison of small molecules
  based on structural similarity, chemical scaffolds, or learned
  representations.

  The primary purpose of this skill is to reduce molecular complexity
  by organizing large libraries into coherent groups that reflect
  shared chemical features or design hypotheses. These groupings
  enable systematic exploration, diversity assessment, redundancy
  reduction, and informed decision-making in downstream workflows.

# =========================
# Intent & Scope
# =========================

intent_scope:
  - clustering molecules by structural similarity or fingerprints
  - grouping compounds by shared scaffolds or cores
  - analyzing chemical diversity within libraries
  - identifying representative molecules from clusters
  - validating library composition and coverage
  - supporting SAR and lead series analysis

out_of_scope:
  - de novo molecule generation or editing
  - binding affinity prediction or docking
  - property prediction or ADMET modeling
  - reaction prediction or retrosynthesis
  - molecular dynamics or physics-based simulation

# =========================
# Input / Output Contract
# =========================

input_types:
  - small_molecule_smiles
  - molecular_library_csv
  - fingerprint_representations
  - scaffold_definitions

output_types:
  - clustered_molecule_groups
  - similarity_matrices
  - scaffold_clusters
  - representative_compound_sets
  - validation_reports

# =========================
# Skill Selection Rules
# =========================

selection_rules:

  select_this_skill_when:
    - the user requests grouping, clustering, or organization of molecules
    - chemical diversity or redundancy needs to be assessed
    - scaffold-based analysis is required
    - SAR exploration or lead series comparison is needed
    - large libraries must be reduced to manageable subsets

  do_not_select_when:
    - the user asks for molecule generation or optimization
    - binding affinity or interaction prediction is the goal
    - physicochemical or ADMET properties are being predicted
    - reaction planning or synthesis reasoning is required

# =========================
# Reasoning Characteristics
# =========================

reasoning_mode:
  - comparative
  - statistical
  - pattern_based

assumptions:
  - input molecules are chemically valid
  - similarity metrics reflect meaningful chemical relationships
  - clustering results are abstractions, not absolute truths
  - downstream interpretation depends on domain context

failure_modes:
  - misleading clusters due to inappropriate similarity metrics
  - over-clustering or under-clustering of chemical space
  - loss of rare but important chemotypes
  - misinterpretation of cluster boundaries

# =========================
# Inter-skill Relationships
# =========================

upstream_skills:
  - molecular_generation_and_optimization
  - screening_and_filtering
  - property_prediction_and_modelling

downstream_skills:
  - lead_optimization
  - interaction_and_binding_design
  - decision_support_and_prioritization

complementary_skills:
  - screening_and_filtering
  - property_prediction_and_modelling

# =========================
# Operational Notes
# =========================

notes:
  - This skill is primarily analytical and organizational in nature,
    serving as a bridge between molecule generation and decision-making.
  - Clustering should be used to inform exploration strategy rather
    than to enforce rigid classifications.
  - The choice of similarity metric or scaffold definition strongly
    influences outcomes and must align with project goals.
  - Outputs from this skill are most powerful when combined with
    property, activity, or interaction data in later stages.


# =========================
# Tools
# =========================
tools:

### Butina Clustering

This tool enables **systematic clustering of small molecules** based on structural or scaffold-level similarity using the Butina algorithm. By grouping chemically related molecules into clusters, it allows the agent to reason about chemical diversity, redundancy, and coverage within large molecular libraries.

The Butina Clustering tool supports both **full-molecule similarity clustering** and **scaffold-centric clustering** through Murcko–Butina mode. This dual capability makes it suitable for workflows ranging from broad chemical space analysis to focused scaffold exploration, depending on whether substituent variation or core structure is the primary concern.

This capability is especially useful in **library triage and analysis pipelines**, where thousands of molecules must be reduced into representative subsets without losing chemical diversity. By applying similarity cutoffs and optional sampling strategies, the tool helps identify cluster representatives, eliminate redundant compounds, and support rational selection for downstream optimization or screening.

Conceptually, this tool operates as an **organizational and validation layer** rather than a generative or predictive engine. It does not create new molecules or infer biological activity; instead, it imposes structure on existing chemical data, enabling informed decisions about which molecules to keep, discard, or prioritize.

Outputs from the Butina Clustering tool should be interpreted as **analytical groupings**, whose usefulness depends on the chosen similarity metric, cutoff threshold, and clustering mode.



### K-Means Clustering

This tool enables **fixed-partition clustering of small molecules** using the k-means algorithm, grouping compounds into a predefined number of clusters based on selected chemical feature representations. Unlike similarity-threshold–based methods, k-means enforces a global partitioning of chemical space, ensuring every molecule is assigned to exactly one cluster.

The K-Means Clustering tool is intended for workflows where **explicit control over the number of groups** is required, such as dataset segmentation, balanced diversity analysis, or comparative studies across predefined chemical regions. By allowing the choice between descriptor-based or fingerprint-based features, the tool supports both physicochemical and structural perspectives on molecular similarity.

This capability is particularly useful in **exploratory analysis and dataset organization**, where the goal is not redundancy elimination but structured partitioning of a molecular library into interpretable subsets. Optional dimensionality reduction enables visualization and inspection of cluster distributions, aiding human-in-the-loop analysis.

Conceptually, this tool functions as a **global clustering mechanism** rather than a chemical similarity filter. It does not infer biological activity, optimize molecules, or generate new structures. Instead, it provides an analytical grouping that can inform downstream prioritization, validation, or modeling decisions.

Outputs from the K-Means Clustering tool should be interpreted as **model-dependent partitions**, whose meaning depends strongly on feature choice and the specified number of clusters.



### SMILES Validation

This tool provides **syntactic validation of SMILES strings**, determining whether a given text representation conforms to valid SMILES grammar. Its sole purpose is to act as a lightweight gatekeeper before molecules are passed into downstream chemical reasoning, modeling, or generation workflows.

The SMILES Validation tool is intended for **early-stage input sanitation and error prevention**. By verifying that an input string is a valid SMILES, it helps prevent silent failures, tool crashes, or misleading results caused by malformed molecular representations.

This capability is especially important in **automated pipelines**, user-facing systems, or multi-step workflows where SMILES may originate from free-text input, external datasets, or generative models. It ensures that only syntactically valid molecules proceed further into the skill and tool chain.

Conceptually, this tool performs **no chemical reasoning or property inference**. It does not assess biological relevance, drug-likeness, or structural quality. It simply answers whether the input string is a valid SMILES encoding.

Outputs from this tool should be treated as **binary validation signals**, typically used to filter, reject, or request correction of invalid molecular inputs before more expensive or complex tools are invoked.


### Multi-SMILES Detection

This tool determines whether a given SMILES string represents **multiple molecular fragments** rather than a single contiguous molecule. It inspects the SMILES syntax to identify fragment separators (typically `.`), which indicate the presence of salts, mixtures, counter-ions, or multi-component systems.

The Multi-SMILES Detection tool is intended for **early structural sanity checks** in small-molecule workflows. Many downstream tools implicitly assume a single molecular entity; passing multi-fragment SMILES into such tools can lead to ambiguous results, incorrect predictions, or silent failures.

This capability is particularly important when handling **raw datasets, user-provided input, or generated molecules**, where salts or solvent fragments may be present. By flagging multi-component SMILES, the agent can decide whether to reject the input, normalize it, or extract a primary fragment before continuing.

Conceptually, this tool performs **pure structural pattern detection**. It does not judge chemical validity, biological relevance, or fragment importance. It only answers whether the SMILES encodes more than one molecular component.



### Largest Fragment Extraction

This tool extracts the **largest chemically valid molecular fragment** from a multi-component SMILES string. When a SMILES encodes multiple disconnected components—such as salts, solvents, or counter-ions—this tool identifies and returns the dominant molecular entity.

The Largest Fragment Extraction tool is intended for **normalization and cleanup workflows**, where downstream tools require a single primary molecule. By isolating the largest fragment, it enables consistent handling of datasets that include auxiliary components without discarding the entire input.

This capability is especially useful after **multi-SMILES detection**, allowing the agent to resolve ambiguity by selecting a representative structure for modeling, screening, or optimization. It supports workflows that prefer pragmatic simplification over strict rejection of multi-component inputs.

Conceptually, this tool performs **fragment selection, not chemical reasoning**. The “largest” fragment is determined by size or atom count, not biological relevance, binding potential, or functional importance. The result should therefore be treated as a normalized proxy rather than a chemically curated truth.


### CAS Number Validation

This tool verifies whether a given text string conforms to the **formal syntactic structure of a CAS Registry Number**. It checks formatting rules such as digit grouping and delimiter placement to determine whether the identifier matches the expected CAS pattern.

The CAS Number Validation tool is intended for **identifier disambiguation and input classification**. In many workflows, user input may ambiguously represent a SMILES string, compound name, or registry identifier. This tool helps the agent determine whether the input should be treated as a CAS number rather than a molecular structure.

This capability is especially useful in **user-facing systems** and automated pipelines that support multiple molecular input modalities. By correctly identifying CAS-formatted inputs, the agent can route them toward appropriate resolution or lookup workflows instead of structural modeling tools.

Conceptually, this tool performs **format validation only**. It does not confirm that the CAS number exists, is registered, or corresponds to a specific molecule. It simply evaluates whether the input matches the CAS numbering pattern.



### Identifier-to-SMILES Resolution

This tool resolves **non-structural chemical identifiers**—such as compound names, CAS numbers, InChI keys, or molecular formulas—into **canonical SMILES representations**. It acts as a bridge between human-readable or registry-based identifiers and structure-centric computational workflows.

The Identifier-to-SMILES Resolution tool is intended for **input normalization and disambiguation**. In many real-world scenarios, users do not provide SMILES directly; instead, they reference compounds using names, registry identifiers, or shorthand descriptors. This tool enables the agent to convert such inputs into a standardized molecular representation suitable for downstream modeling, screening, or analysis.

Beyond simple conversion, the tool also provides **cross-check metadata**, including synonyms, CAS mappings, and service provenance. This allows the agent to assess resolution confidence, detect ambiguity, and decide whether additional validation or user clarification is required.

Conceptually, this tool performs **identifier resolution, not chemical reasoning**. It does not evaluate molecular properties, optimize structures, or assess biological relevance. Its sole purpose is to translate identifiers into structural form while exposing contextual metadata that supports reliable downstream decision-making.


