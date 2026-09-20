# SCOUT — Supply Chain Operation and Vulnerability Threat Assessment

SCOUT is a master's integrative project developed within the **Master's in Applied Artificial Intelligence at Tecnológico de Monterrey** under the supervision of an investigator from the **MIT Center for Transportation & Logistics (MIT CTL)**.

The project explores how cybersecurity vulnerability information can be translated into **operational priorities for supply-chain decision makers**. Rather than presenting technical severity scores alone, SCOUT is intended to connect cyber conditions with the processes, dependencies, vendors, recovery capabilities, and physical operations that may be affected.

## Project purpose

Modern supply chains depend on connected technologies such as ERP, WMS, TMS, cloud services, automation, industrial equipment, remote vendor access, and other third-party systems. A technical vulnerability can therefore have consequences that extend beyond IT and affect production, logistics, quality, safety, customer service, or recovery time.

SCOUT is being designed as an **interactive, AI-enabled decision-support and learning application** that helps users:

- assess cybersecurity posture and relevant vulnerabilities in operational context;
- identify which supply-chain processes, assets, vendors, or dependencies may require attention first;
- understand why a cyber condition matters from an operational perspective;
- visualize exposure through a Value Stream Map (VSM) and prioritized heat-map-style outputs;
- receive contextual explanations and recommended next actions;
- learn cybersecurity concepts while completing the assessment; and
- support repeated assessments as conditions, vulnerabilities, and threat intelligence change.

SCOUT is **not intended to replace a formal cybersecurity audit, penetration test, vulnerability scanner, SIEM platform, or incident-response function**. The initial project is a research-oriented prototype and decision-support tool.

## Intended users

The current user groups considered for SCOUT include:

- supply-chain engineers and logistics professionals;
- production and manufacturing personnel; and
- IT / cybersecurity personnel.

The application is expected to support different levels of cybersecurity knowledge through role-aware questions, explanations, and assessment paths.

## Conceptual workflow

The proposed end-to-end workflow is:

1. **User role and knowledge baseline**
2. **Guided organizational assessment**
3. **Supply-chain / process / asset mapping**
4. **Cybersecurity and vulnerability context**
5. **Transparent risk-prioritization logic**
6. **Interactive Value Stream Map (VSM)**
7. **Prioritized findings and report**
8. **Contextual learning and explanations**
9. **Pilot evaluation / reassessment**

The project follows a **backbone-first** strategy: establish a complete, auditable assessment-to-report workflow before adding more advanced AI, gamification, autonomous agents, or broad enterprise integrations.

## Proposed risk model

SCOUT is being designed to distinguish technical vulnerability information from operational consequence.

### Cyber / threat inputs

Potential inputs include:

- CVE identifiers;
- CVSS v4.0 metrics;
- CISA Known Exploited Vulnerabilities (KEV) status;
- EPSS exploitation probability;
- asset exposure and exploitability information;
- vendor advisories and other approved threat-intelligence sources; and
- relevant risk-prioritization factors from CISA BOD 26-04.

### Operational inputs

Potential operational factors include:

- process criticality;
- vendor and third-party dependence;
- redundancy and available workarounds;
- recovery capability and recovery time;
- business / downtime impact;
- safety and physical consequences;
- dependency and concentration risk; and
- potential propagation to upstream or downstream operations.

### Proposed outputs

The current conceptual model separates:

- **Cyber Threat Urgency** — technical and threat-based priority;
- **Operational Consequence** — potential impact on mapped operations;
- **SCOUT Operational Priority** — combined decision-support result; and
- **Assessment Confidence** — completeness and quality of the information supporting the result.

The authoritative scoring methodology is intended to remain **transparent, reproducible, and auditable**. Generative AI may support explanation, summarization, contextual learning, and knowledge retrieval, but it should not independently assign the authoritative risk score unless a separately validated method is later approved.

## Minimum Viable Product direction

The current MVP direction includes:

- guided assessment questionnaire;
- transparent risk-prioritization engine;
- basic interactive VSM / supply-chain visualization;
- prioritized findings and rationale;
- frontend, backend, and persistent database;
- minimal contextual educational content; and
- baseline measurement for later user evaluation.

Advanced capabilities such as expanded RAG, automated threat-intelligence ingestion, autonomous agents, push notifications, complete gamification, and multi-user organizational collaboration remain conditional on scope, technical feasibility, and available project time.

## Candidate data sources

SCOUT may use several types of data:

- **Public vulnerability intelligence:** CVE/NVD records, CVSS, CISA KEV, EPSS, and vendor advisories.
- **Organizational assessment data:** process, asset, dependency, control, recovery, vendor, and impact information supplied by users.
- **Synthetic scenarios:** controlled development and demonstration cases when real organizational data are unavailable or inappropriate to share.
- **MIT incident / observatory data:** potential research source; access, schema, and permitted use remain subject to confirmation.
- **Human pilot data:** knowledge responses, task performance, usability feedback, and selected interaction metrics, subject to the applicable approval and data-handling requirements.

## Repository structure

The initial repository is expected to evolve approximately as follows:

```text
SCOUT/
│
├── data/
│   └── .gitkeep
│
├── notebooks/
│   └── .gitkeep
│
├── documentation/
│   ├── .gitkeep
│   └── deliverables/
│       ├── week-01/
│       │   └── .gitkeep
│       ├── week-02/
│       │   └── .gitkeep
│       ├── week-08/
│       │   └── .gitkeep
│       └── week-09/
│           └── .gitkeep
│
└── README.md
```

Credentials, API keys, passwords, tokens, confidential company information, and unauthorized data must **not** be committed to the repository.

## Research direction

The current research direction includes questions such as:

- How can technical cybersecurity vulnerability information be translated into explainable operational priorities for supply-chain processes?
- How does adding operational context change prioritization relative to technical severity alone?
- Can contextualized cybersecurity learning improve users' understanding or decision-making during the assessment?

These questions and the final experimental design remain subject to investigator and academic validation.

---

# References used to support SCOUT so far

The sources below have informed the project's conceptual design, assessment methodology, vulnerability-prioritization approach, resilience questions, and learning-assessment strategy. This list is a **working reference set**, not the final literature review.

## Core vulnerability and threat-prioritization references

1. **Forum of Incident Response and Security Teams (FIRST). Common Vulnerability Scoring System (CVSS) v4.0 — Specification Document.**  
   Defines the CVSS v4.0 Base, Threat, Environmental, and Supplemental metric groups used to characterize vulnerability severity and context.  
   https://www.first.org/cvss/v4.0/specification-document

2. **FIRST. Common Vulnerability Scoring System (CVSS) v4.0 — User Guide.**  
   Provides interpretation guidance and emphasizes that the CVSS Base Score measures severity and should not be treated as a complete risk score by itself.  
   https://www.first.org/cvss/v4.0/user-guide

3. **FIRST. Exploit Prediction Scoring System (EPSS).**  
   Provides a data-driven estimate of the probability that a published CVE will experience observed exploitation activity in the next 30 days.  
   https://www.first.org/epss/

4. **FIRST. Using EPSS.**  
   Provides guidance on interpreting EPSS as an exploitation-likelihood signal rather than as a severity or complete risk score.  
   https://www.first.org/epss/using-epss

5. **Cybersecurity and Infrastructure Security Agency (CISA). BOD 26-04 — Prioritizing Security Updates Based on Risk.**  
   Used as a reference for risk-informed vulnerability prioritization, including factors such as exposure, known exploitation, exploit automation, and technical impact.  
   https://www.cisa.gov/news-events/directives/bod-26-04-prioritizing-security-updates-based-risk

6. **CISA. Known Exploited Vulnerabilities (KEV) Catalog.**  
   Provides an authoritative catalog of vulnerabilities with evidence of exploitation in the wild and is considered as an input to vulnerability prioritization.  
   https://www.cisa.gov/known-exploited-vulnerabilities-catalog

7. **National Institute of Standards and Technology (NIST). National Vulnerability Database (NVD).**  
   Provides standards-based vulnerability-management data and enrichment for published CVEs, including CVSS, CWE, CPE applicability, and related references.  
   https://nvd.nist.gov/

8. **NIST. NVD Vulnerabilities API.**  
   Candidate machine-readable source for future automated ingestion of vulnerability records into SCOUT.  
   https://nvd.nist.gov/developers/vulnerabilities

## Cybersecurity risk, resilience, and supply-chain references

9. **NIST. The NIST Cybersecurity Framework (CSF) 2.0. NIST CSWP 29, 2024.**  
   Provides the broader cybersecurity-risk structure used to organize outcomes across Govern, Identify, Protect, Detect, Respond, and Recover.  
   https://doi.org/10.6028/NIST.CSWP.29

10. **Boyens, J., Smith, A., Bartol, N., Winkler, K., Holbrook, A., & Fallon, M. NIST SP 800-161 Rev. 1 — Cybersecurity Supply Chain Risk Management Practices for Systems and Organizations.**  
    Supports the treatment of supplier, third-party, technology, and supply-chain cybersecurity risks and includes an SCRM assessment scoping questionnaire.  
    https://doi.org/10.6028/NIST.SP.800-161r1-upd1

11. **NIST SP 800-34 Rev. 1 — Contingency Planning Guide for Federal Information Systems.**  
    Used as a reference for business impact analysis, recovery planning, resilience, and concepts such as recovery objectives and disruption consequences.  
    https://doi.org/10.6028/NIST.SP.800-34r1

12. **International Society of Automation (ISA). ISA/IEC 62443 Series of Standards.**  
    Provides cybersecurity principles and requirements for industrial automation and control systems and is relevant to SCOUT's manufacturing / OT context.  
    https://www.isa.org/standards-and-publications/isa-standards/isa-iec-62443-series-of-standards

13. **MIT Center for Transportation & Logistics. Supply Chain Cybersecurity Lab.**  
    Provides the direct research context for SCOUT, including supply-chain cyber vulnerability mapping, technology/data/physical dependencies, operational impact, recovery, and systemic / concentration risk.  
    https://ctl.mit.edu/research/supply-chain-cybersecurity-lab

## Learning-assessment and questionnaire-design references

14. **Sherman, A. T., et al. (2019). The CATS Hackathon: Creating and Refining Test Items for Cybersecurity Concept Inventories.**  
    Used as a reference for scenario-based cybersecurity assessment items, question stems, plausible distractors, and concept-focused evaluation.  
    https://arxiv.org/abs/1901.09286

15. **Sherman, A. T., et al. (2020). Experiences and Lessons Learned Creating and Validating Concept Inventories for Cybersecurity.**  
    Supports the design and validation of cybersecurity concept inventories through expert review, misconception analysis, scenario development, and psychometric testing.  
    https://arxiv.org/abs/2004.05248

## Reference-management note

The final academic report should standardize these references into the citation style selected for the master's course (e.g., APA 7 or IEEE) and should distinguish:

- sources used to define the **risk methodology**;
- sources used as **live data / intelligence inputs**;
- sources used for **questionnaire and learning-measurement design**; and
- sources used only as **background or research context**.

---

## Current project status

SCOUT remains in the project-definition and methodology-design stage. The exact unit of analysis, final scoring logic, weighting rules, automated data-ingestion scope, ML research target, and pilot-study design are still being refined and require validation before they should be treated as final project specifications.
