# Skill: Screening and Filtering

## Domain
Small Molecule Design

## Cluster
Tools for similarity-based, pharmacophore-based, substructure-based, novelty-based, or rule-based screening and filtering of small-molecule libraries.

## Skill Description

Screening and Filtering focuses on **reducing, prioritizing, and organizing molecular libraries** by applying comparison, matching, and exclusion criteria. Unlike property prediction, which evaluates individual molecules in isolation, this skill operates in a **comparative and selection-driven mode**, determining which molecules should be retained, deprioritized, or discarded from larger sets.

This skill is invoked when the agent must **identify molecules that resemble known actives**, satisfy pharmacophore hypotheses, contain or exclude specific substructures, or meet novelty and diversity constraints. It is especially important in workflows where large libraries are generated upstream and must be narrowed down efficiently before deeper analysis or optimization.

Screening and Filtering does not modify molecular structures or attempt to improve them. Instead, it applies **rules, similarity metrics, pattern matching, or hypothesis-based constraints** to existing molecules. The outputs are ranked or filtered subsets of the original library, often annotated with similarity scores, match flags, or classification labels.

Conceptually, this skill acts as the **library triage and prioritization layer** of small-molecule pipelines. It ensures that computational and experimental resources are focused on the most promising candidates by eliminating redundancy, low-relevance compounds, or undesired chemical patterns early in the workflow.

## When to Use This Skill

Use this skill when:
- large molecular libraries need to be reduced or prioritized
- similarity to known ligands or reference molecules must be assessed
- pharmacophore hypotheses should be used to filter candidates
- substructure inclusion or exclusion rules are required
- novelty, diversity, or redundancy control is important
- downstream optimization or docking needs a smaller, higher-quality set

## When *Not* to Use This Skill

Do **not** use this skill when:
- the task is to generate new molecules
- molecular properties need to be predicted quantitatively
- lead optimization or structural editing is required
- docking, molecular dynamics, or binding simulations are requested
- retrosynthesis or reaction planning is the goal

## Conceptual Role in the System

Within the agent architecture, Screening and Filtering serves as the **selection and pruning layer** between generation, prediction, and optimization. It transforms large, unstructured molecule sets into focused candidate lists suitable for deeper evaluation.

This skill is commonly used:
- downstream of **Molecular Generation and Optimization**
- after **Property Prediction and Modeling** to enforce thresholds
- upstream of **Docking, Simulation, or Lead Optimization**
- to control diversity and novelty across iterative design cycles

By separating screening logic from generation and prediction, the system maintains clear reasoning boundaries and avoids conflating evaluation with creation.

## Expected Outputs

Depending on the selected tool, this skill may produce:
- filtered subsets of molecular libraries
- ranked candidate lists with similarity or match scores
- novelty or diversity annotations
- pass/fail flags based on defined screening criteria
- structured CSV or tabular outputs for downstream workflows

# =========================
# Tools
# =========================
tools:
### Similarity-Based Molecular Screening

This tool enables **screening of molecular libraries based on structural similarity** to one or more reference molecules. It is designed for workflows where the agent must identify compounds that are chemically close to known ligands, hits, or leads.

The Similarity Screening tool is intended for scenarios such as **analogue selection, lead hopping, library pruning, or hit expansion**, where chemical resemblance is used as a proxy for shared biological or physicochemical behavior. By comparing molecules using fingerprint-based similarity metrics, the tool allows rapid filtering of large libraries to retain only compounds above a defined similarity threshold.

Unlike property prediction or optimization tools, similarity screening operates in a **comparative and reference-driven mode**. It does not evaluate molecules in isolation; instead, it measures how closely each library molecule matches a provided reference structure or reference set.

Conceptually, this tool represents the **chemical proximity filter** within the Screening and Filtering skill. It is most effective when reliable reference molecules are available and when maintaining chemical continuity with known compounds is desirable.
### Pharmacophore-Based Molecular Screening

This tool enables **screening of molecular libraries against a predefined pharmacophore hypothesis**. It is designed for workflows where binding requirements have already been abstracted into a set of interaction features, and molecules must be filtered based on their ability to satisfy those features.

The Pharmacophore Screening tool is intended for **hypothesis-driven filtering**, where chemical structures are evaluated not by overall similarity, but by their capacity to reproduce key spatial and functional interactions. Typical use cases include filtering large libraries after pharmacophore hypothesis generation, validating whether generated molecules respect essential binding constraints, or prioritizing candidates that align with known interaction patterns.

Unlike similarity screening, which operates on 2D structural resemblance, pharmacophore screening performs **feature-level matching**, often in three-dimensional space. This allows chemically diverse scaffolds to pass the screen as long as they satisfy the required interaction features.

Conceptually, this tool represents the **interaction-consistency filter** within the Screening and Filtering skill. It ensures that retained molecules adhere to known or hypothesized binding requirements before further evaluation or optimization.
### Substructure and Toxicophore Screening

This tool enables **rule-based structural filtering** of molecular libraries by detecting the presence or absence of predefined substructures or toxicophores. It is designed for workflows where chemical patterns must be explicitly enforced or eliminated before further analysis.

Substructure Screening operates on **exact or graph-based matching**, allowing molecules to be screened IN or OUT based on whether they contain specific fragments, functional groups, or toxic alerts. This makes it especially valuable for toxicity elimination, scaffold enforcement, medicinal chemistry rules, and safety-driven filtering.

Unlike similarity or pharmacophore screening, this tool does not evaluate global resemblance or interaction features. Instead, it applies **binary structural logic**: a molecule either contains the specified pattern or it does not. As a result, it is often used early in screening pipelines to remove chemically undesirable candidates or to enforce mandatory substructures.

Conceptually, this tool represents the **hard-constraint filter** within the Screening and Filtering skill. It ensures that all retained molecules conform to explicit structural rules before downstream property prediction, optimization, or docking.
### Pairwise Molecular Similarity Computation

This tool computes the **pairwise structural similarity** between two individual small molecules using the Tanimoto coefficient derived from molecular fingerprints.

Unlike library-level screening tools, this operation is **local, atomic, and diagnostic**. It evaluates similarity between exactly two molecules and returns a single similarity score, without performing filtering, ranking, or database-wide comparisons.

The tool is primarily used as a **building block** inside larger workflows such as:
- validating similarity thresholds
- debugging screening results
- comparing generated molecules against a specific reference
- computing reward signals or constraints in optimization loops

Conceptually, this tool answers the question:  
*“How similar are these two molecules, numerically?”*

It does **not** decide whether a molecule should be kept or discarded, and it does **not** perform novelty, screening, or clustering by itself. Those higher-level decisions belong to screening or optimization skills that may internally rely on this similarity computation.
