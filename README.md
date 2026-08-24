# Lyme Disease Controversy: Mapping Positions, Evidence, and Claim Translation

This repository supports a connected programme of research examining how scientific disagreement about persistent symptoms attributed to Lyme disease is represented, organised, and translated through the scholarly literature.

The first study developed and validated a hybrid AI–human scientometric workflow for mapping positions, themes, publication venues, and citation patterns across 25 years of literature. The second study builds on that corpus and classification framework by asking which evidence domains underpin the competing positions and how selected clinical and preclinical findings are extended into broader mechanistic and treatment claims.

The broader research programme is moving from **claim orientation**, to **evidence domain**, to **evidence uptake and claim translation**. Current follow-up work examines which primary studies are selected by reviews and guidelines and how findings are preserved, generalised, qualified, or extended as they move through the evidence pathway. This work maps the structure of disagreement and the points at which additional research could make disputed claims more testable; it does not provide individual treatment advice or treat literature mapping as an adjudication of biomedical truth.

The research programme and this supporting repository were presented at the [1st Annual Conference on Chronic Infection Pathologies (CIP 2025)](https://www.tezted.com/cip2025), held in Jyväskylä, Finland, on 5–7 September 2025. The [conference programme](https://www.tezted.com/_files/ugd/689421_8951ebd9718d40ab85938623b466c577.pdf) included Teo Susnjak's presentation, *Revisiting the Lyme Narrative: AI Exposes Fault Lines in Chronic Infection Debates*. The next stage, following evidence into citations and claims, is scheduled for presentation at the [2nd Annual Conference on Chronic Infection Pathologies (CIP 2026)](https://www.chronicinfectionpathologies.com/), held in Helsinki, Finland, on 4–6 September 2026.

## Studies

| Study | Focus | Repository materials |
| --- | --- | --- |
| [Study 1: Scientometric mapping](study-1/) | Positions, themes, venues, citation patterns, and validation of a hybrid AI–human workflow | Original prompts and inter-rater training material |
| [Study 2: Evidence-domain translation](study-2/) | Evidence domains, stance orientation, clinical-trial appraisal, and preclinical evidence translation | Selected copyright-safe data, analysis code, and compact worked examples as they become available |

## Study 1

### Large language models for scientometric mapping of scientific controversy: A validated hybrid AI–Human framework

**Authors:** Teo Susnjak, Cole Palffy, Tatiana Zimina, Nazgul Altynbekova, Kunal Garg, and Leona Gilbert

- [Preprint on medRxiv](https://www.medrxiv.org/content/10.1101/2025.04.03.25325216v1)
- [Published article in *Scientometrics*](https://link.springer.com/article/10.1007/s11192-026-05681-3)
- [DOI: 10.1007/s11192-026-05681-3](https://doi.org/10.1007/s11192-026-05681-3)

### Published abstract

This study advanced large language model (LLM)-enabled scientometric methods for analysing contested scholarly communication through a hybrid AI–human workflow for large-scale mapping of stance and themes in scientific corpora. We treated the literature as a large textual record and examined how competing knowledge claims were distributed across venues and over time.

We used the Lyme disease controversy as a testbed, applied multiple LLMs to thousands of abstracts, and validated the outputs with domain experts to trace longitudinal shifts in authorial stance and thematic focus. We examined how alternative perspectives were distributed across venues and themes, how these distributions changed over time, and how venue stratification shaped the visibility of different epistemic positions.

Results showed persistent asymmetries in publication placement and citation attention, with higher-impact venues disproportionately hosting one perspective relative to the other, while thematic mapping identified recurring fault lines around disputed claims. Validation showed substantial alignment between LLM outputs and expert ratings across classification tasks (Cohen’s kappa 0.58–0.71), broadly comparable to expert–expert agreement and sufficient for corpus-level scientometric analysis.

The framework offers a reproducible model of human–LLM integration in literature-based scientometrics and enables venue- and time-resolved indicators for monitoring controversies and publication imbalances.

## Study 2

### Evidence-Domain Translation in the Lyme Disease Controversy over Persistent Post-Treatment Symptoms

**Authors:** Teo Susnjak, Lorenz Brehme, Kunal Garg, Gordana Avramovic, and Leona Gilbert

- [Article and abstract at *Frontiers in Cellular and Infection Microbiology*](https://www.frontiersin.org/journals/cellular-and-infection-microbiology/articles/10.3389/fcimb.2026.1913455/abstract)
- [DOI: 10.3389/fcimb.2026.1913455](https://doi.org/10.3389/fcimb.2026.1913455)

### Abstract

The debate between post-treatment Lyme disease syndrome (PTLDS) and chronic Lyme disease (CLD) reflects different views about the causes and treatment of persistent symptoms attributed to Lyme disease, including symptoms that continue after recommended antibiotic treatment. This study examines how competing positions in the controversy draw on different scientific evidence domains, and how findings from those domains are extended into broader clinical and mechanistic claims. We analysed a 2000–2024 literature corpus using a prompt-optimised three-model large language model ensemble to classify abstracts by stance, theme and model-derived study design. We then conducted targeted full-text audits of selected retreatment and treatment-duration trials, and preclinical persistence studies.

Across the higher-volume evidence domains, model-derived claim orientations differed by study-design tier. Observational studies and commentaries showed PTLDS-oriented distributions, whereas case reports or series, animal models, and *in vitro* studies showed CLD-oriented distributions. The smaller RCT and guideline tiers were also PTLDS-oriented, but the small numbers and wide confidence intervals made this direction uncertain.

The clinical trial audit showed that some trials reported short-term or symptom-specific improvements, while the interpretation of these findings depended on their durability, endpoint consistency, eligibility criteria, treatment burden, and safety. The citation-conditioned preclinical audit identified findings that support several candidate mechanisms and generate hypotheses for further study, but these findings did not by themselves establish viable infection as the cause of persistent human symptoms or demonstrate the efficacy of prolonged antimicrobial treatment.

The findings show a recurring distinction between mechanism-level evidence and the evidence needed to establish patient-level causation or durable clinical benefit. They map how these different forms of evidence are distributed and translated across the PTLDS–CLD literature.

## Data and copyright

The repository does not redistribute the corpus of article abstracts or copyrighted full texts. Selected raw data not subject to third-party copyright, together with analysis code and compact worked examples, will be made available under [Study 2](study-2/). Persistent identifiers will be retained where possible so that source records can be retrieved from their original databases or publishers.
