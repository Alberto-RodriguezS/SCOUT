# MIT TRACE — Master Incident Research, Eligibility, Classification, Reliability and Enrichment Specification

## 1. Purpose and scope

This document defines the conceptual and operational rules that govern how MIT TRACE researches, qualifies, represents, validates, enriches, and prepares cybersecurity incidents for inclusion in the platform.

MIT TRACE is designed to study cyber incidents that cause material consequences to enterprise operations, supply-chain activity, value-delivery processes, or materially dependent organizations. Its purpose is not to maintain a general-purpose catalog of cyberattacks, data breaches, threat actors, vulnerabilities, or security events. A cyber event becomes relevant to TRACE only when the available evidence establishes a material business-flow consequence within the scope defined in this specification.

This specification serves as the source of truth for:

- Research Request interpretation and normalization.
- Incident eligibility.
- Evidence acquisition and source hierarchy.
- Controlled-vocabulary classification.
- Structured incident records.
- Contextual inference.
- Duplicate and expansion control.
- Incident reliability assessment.
- Enrichment and information completion.
- Severity estimation.
- Analyst review and incident lifecycle decisions.

The document intentionally separates conceptual rules from implementation-specific prompt wording. Model instructions, orchestration code, provider configuration, search-engine configuration, and internal prompt templates may evolve without changing the semantic requirements defined here.

The explicit values of the controlled catalogs are not enumerated in the main body. Dedicated insertion blocks are provided in Section 16 so the frozen catalog values can be maintained independently from the interpretation rules.

---

## 2. Core TRACE incident model

### 2.1. Central incident invariant

Every TRACE incident is organized around one directly attacked organization:

> **`TargetedCompany` is the organization that directly received the cybersecurity attack and from which the incident under analysis originates.**

This identity must remain stable throughout research, extraction, reliability assessment, enrichment, deduplication, analyst review, and publication.

Organizations that experience consequences because of the same incident are not substitutes for `TargetedCompany`. They are represented as materially affected related organizations through `SupplyChainRelations`.

### 2.2. What TRACE is intended to capture

TRACE is concerned with the propagation of cyber risk into real business activity. Relevant consequences include, among others:

- Interruption or degradation of production.
- Inability to deliver a service.
- Shutdown of facilities or operational systems.
- Impairment of orders, transactions, checkout, or sales processing.
- Procurement or sourcing disruption.
- Warehousing or inventory disruption.
- Transportation or logistics delay.
- Distribution or fulfillment interruption.
- Backlog or capacity loss.
- Disruption of business-critical value-delivery processes.
- Material effects on customers, suppliers, service recipients, or other dependent organizations caused by the same cyber incident.

The source does not need to use the words *supply chain* or any TRACE catalog code. Eligibility and classification are semantic and evidence-based.

### 2.3. What TRACE does not treat as sufficient by itself

The following facts do not independently establish TRACE eligibility:

- A data breach or data exposure.
- Credential theft.
- Unauthorized access.
- Malware infection.
- Ransomware deployment.
- Extortion.
- DDoS activity.
- Financial theft.
- Regulatory or reputational consequences.
- A threat-actor claim.
- The existence of a cybersecurity investigation.
- Compromise of an ERP, OT/ICS platform, portal, plant network, warehouse system, or other technology without evidence that the corresponding business activity was materially impaired.
- A vulnerability for which successful exploitation and material consequence have not been established.

These facts may form part of a qualifying incident, but they cannot replace the material-consequence requirement.

---

## 3. Incident eligibility standard

### 3.1. Two required predicates

A final TRACE incident requires positive evidence of both of the following predicates:

1. **Cyber-event predicate:** a cybersecurity event affected the directly attacked `TargetedCompany`.
2. **Material-consequence predicate:** the same event produced a material consequence to the `TargetedCompany`'s enterprise operations, supply-chain flow, or value-delivery activity, or materially affected another organization's corresponding activity through the same incident.

If either predicate cannot be supported, the candidate is not eligible for final inclusion.

Absence of evidence that an incident is out of scope is not evidence that it is in scope.

### 3.2. Materiality test

Materiality is established through observable business consequences rather than through the perceived importance of the attacked technology or organization. Relevant evidence may include:

- Production lines, plants, warehouses, stores, ports, offices, or other facilities stopping or operating at reduced capacity.
- Unavailable or materially degraded services.
- Inability to process orders, transactions, claims, bookings, payments, or customer requests.
- Delayed shipments, cargo, deliveries, sourcing, or replenishment.
- Inventory, warehouse, transportation, or fulfillment disruption.
- Documented backlog, capacity reduction, cancellation, rerouting, or service suspension.
- Material same-incident consequences reported by dependent organizations.

A consequence may be stated explicitly or established through strong incident-specific contextual evidence. Contextual inference is permitted only when the reported facts make the operational mapping defensible.

### 3.3. Context-supported inference

TRACE permits controlled semantic inference because reporting frequently describes operational effects without using catalog terminology.

Examples:

- A source reporting that assembly lines stopped can support a production/manufacturing supply-chain classification.
- A checkout platform outage that prevents purchases can support a sales or order-flow classification.
- Delayed cargo movements can support a distribution, fulfillment, transportation, or logistics classification, depending on the frozen catalog definitions.
- A warehouse-management outage that prevents inventory processing can support a warehousing or inventory classification.

Inference must remain incident-specific. It must not be based solely on the victim's industry, normal business model, attack family, threat actor, facility type, or generic expectations about a compromised technology.

### 3.4. Eligibility is not rescued by classification

Required fields such as `ImpactTypes` or `MostAffectedSupplyChainSection` must describe consequences that are already defensible from the evidence. They must never be invented to preserve a candidate that otherwise fails the eligibility test.

---

## 4. Research Request semantics

A Research Request defines the search scope. It does not predetermine the final classifications of incidents that are found.

### 4.1. Structured request dimensions

The normalized request may define:

- Free-text research objective (`requestText`).
- Target organizational type (`targetType`).
- Target organization name (`targetName`).
- Time range (`from` / `to`).
- Geographic mode and countries.
- Research languages.
- Supply-chain relationship filters.
- Affected-sector filters.
- Affected targeted-company department filters.
- Supply-chain-section filters.
- Attack-type filters.
- Impact-type filters.

Geographic scope may represent a global request or an explicit selected-country set. Country values use ISO-based identifiers.

### 4.2. Structured filters are authoritative after normalization

The request's structured fields define the operative search scope. Free text provides additional context only when compatible with those fields.

A semantic inconsistency between free text and structured filters should be detected during request validation rather than silently resolved through model guesswork. Requests that contain meaningful contradictions should be flagged for review or correction before research proceeds. This is one of the purposes of the semantic validation research step.

### 4.3. Discovery filters versus final classification

Only explicit Research Request selections constrain discovery. Final classification uses the complete relevant frozen catalogs.

For example, a request that filters for one attack type may discover an incident in which the evidence supports additional attack mechanisms. Those additional mechanisms may be included in the final incident record when supported, because request filtering and incident classification answer different questions.

---

## 5. Research and evidence-acquisition lifecycle

TRACE research follows a high-recall discovery and high-precision qualification model.

### 5.1. Discovery

Discovery seeks distinct candidate incidents across the permitted dates, geographies, languages, organization types, sectors, and incident families. Broad requests should preserve breadth rather than repeatedly concentrating on prominent incidents or a single cyberattack family.

Searches may combine cyber-event terminology with business-consequence language such as:

- Disrupted operations.
- Halted production.
- Service unavailable.
- Facility shutdown.
- Delayed shipments.
- Impaired orders.
- Warehouse or inventory disruption.
- Sourcing or procurement disruption.
- Reduced capacity.

Literal use of terms such as *cyberattack*, *ransomware*, or *supply chain* is not required if the incident can be identified through equivalent terminology.

### 5.2. Candidate preservation

During evidence gathering, a lead may remain under investigation when the available context supports:

- A cybersecurity event.
- An identifiable directly attacked organization.
- A plausible material TRACE consequence.
- Relevance to the Research Request.
- At least one retrievable source URL.

Missing non-core fields do not automatically eliminate a lead. Weak sources may justify continued investigation, but they do not lower the evidentiary standard for final inclusion.

### 5.3. Evidence acquisition

Research should actively seek evidence for:

- Incident identity.
- Directly attacked organization.
- Target country.
- Date and timeline.
- Attack mechanism when known.
- Actual operational or supply-chain consequence.
- Affected internal functions.
- Affected supply-chain sections.
- Materially affected related organizations.
- Affected sectors and geographies.
- Financial and duration claims.
- Threat-actor attribution when reported.
- Sufficiently strong URLs for final source assignment.

Research should extend beyond cybersecurity media to company disclosures, securities or regulatory filings, government and CERT publications, court records, major news organizations, financial/business reporting, regional or industry reporting, and established incident-response or cybersecurity research.

### 5.4. Native research planning

The research engine may perform its own planning and decomposition of the normalized Research Request. TRACE does not depend on a separate catalog-specific search-plan schema as part of the conceptual data model. Whatever planning mechanism is used must preserve the normalized request constraints, search breadth, evidence requirements, and duplicate controls defined in this specification.

---

## 6. Evidence and source-quality model

### 6.1. Evidence hierarchy

When claim-relevant evidence is available, sources should generally be prioritized in the following order:

1. Victim or official disclosures, regulatory or securities filings, government/CERT publications, and court records.
2. Reputable independent wire services, major news organizations, business/financial reporting, and investigative reporting.
3. Established incident-specific cybersecurity or incident-response research.
4. Reputable regional or industry reporting.

Blogs, forums, social-media posts, aggregators, reposts, trackers, content farms, SEO summaries, leak sites, and uncorroborated threat-actor claims are low-confidence lead sources. They may initiate further research but should not outrank stronger evidence.

Authority alone is insufficient. A source must actually support the claim for which it is being used.

### 6.2. `MainSources`

`MainSources` contains the strongest available evidence bundle for the incident. Collectively, these URLs should support the core incident identity, the directly attacked `TargetedCompany`, and the material TRACE consequence.

Selection should consider:

- Authority.
- Direct knowledge.
- Incident specificity.
- Coverage of core claims.
- Independence and corroboration.
- Accessibility.

The preferred bundle is the smallest sufficient set, normally one to three URLs.

### 6.3. `AdditionalSources`

`AdditionalSources` contains corroborating or supplementary evidence that is lower priority than `MainSources` or supports secondary details.

A URL must not remain in `AdditionalSources` when it is materially stronger, more authoritative, or more important to the core claims than a selected `MainSource`. Sources must be re-ranked whenever stronger evidence is found.

For a final accepted TRACE record, both `MainSources` and `AdditionalSources` are expected to contain valid, retrievable URLs. An intermediate research or enrichment record may temporarily preserve `NO INFORMATION` when supplementary corroboration has not yet been obtained; such a gap should be treated as unresolved evidence work rather than silently filled.

### 6.4. URL integrity

Only exact URLs obtained from gathered evidence may be stored. TRACE must not invent, reconstruct, normalize into a different URL, or fabricate source locations.

Reposts of the same underlying claim do not constitute independent corroboration.

---

## 7. Structured incident record

Each final incident represents one distinct cyber event. The following fields constitute the principal analytical record.

### 7.1. `incidentId`

Persistent internal identifier for the incident. Existing identifiers must be preserved during reliability checks, enrichment, batch processing, and record updates.

### 7.2. `IncidentTitle`

A concise, source-grounded title that identifies the event without adding unsupported claims, threat-actor attribution, or consequences.

### 7.3. `Summary`

A compact source-grounded description that states:

1. The cybersecurity event.
2. The material operational, supply-chain, or value-delivery consequence that makes the event relevant to TRACE.

The summary must not imply facts that are absent from the gathered evidence.

### 7.4. `TargetedCompany`

The directly attacked organization. TRACE stores a canonical normalized name in uppercase.

Canonicalization should:

- Use the identifiable official organization or brand identity relevant to the incident.
- Remove unnecessary legal/corporate adornments such as `INC`, `CORP`, `COMPANY`, `LTD`, `LLC`, `PLC`, `AG`, `SA`, `SAS`, `GMBH`, `HOLDINGS`, `GROUP`, or leading `THE`, unless inseparable from the recognized identity.
- Remove tickers, exchange labels, country annotations, URLs, domain suffixes, and incidental punctuation.
- Avoid commas when the value is serialized inside CSV-oriented fields.

Examples:

- `Toyota Motor Corporation` → `TOYOTA`
- `The Boeing Company` → `BOEING`
- `Example.com Ltd.` → `EXAMPLE`

Canonicalization must not merge genuinely distinct subsidiaries, business units, or legal entities when the incident evidence identifies them separately.

### 7.5. `CountryTargetedCompany`

ISO-3 country associated with the directly attacked `TargetedCompany` in the specific incident.

This value must not be substituted with:

- A merely affected country.
- The corporate parent's headquarters country.
- A customer country.
- The reporting location.
- A country mentioned only because the organization operates there.

### 7.6. `ApproximateIncidentDate`

Best-supported incident date in `dd/mm/yyyy` form.

The field should reflect the earliest defensible date associated with the incident itself or its material manifestation, rather than the publication date of a source. When reporting supports only an approximate date, uncertainty should be preserved rather than replaced with false precision.

The date is important incident evidence but is not, by itself, the duplicate-analysis identity key.

### 7.7. `AttackDurationDays`

Number of days for which the attack or materially affected period can be defensibly established from the incident timeline. Use `NO INFORMATION` when the available evidence does not support a duration.

Duration must not be estimated from article publication intervals or assumed remediation timelines.

### 7.8. `FinancialImpact`

Financial consequence expressed in USD millions when supported by incident-specific evidence. Use `NO INFORMATION` when the financial effect is unavailable, speculative, or cannot be tied to the incident with sufficient confidence.

### 7.9. `AttackTypes`

Multi-value controlled classification of attack mechanisms, modalities, or recognizable attack archetypes materially present in the incident.

`AttackTypes` answers **how the cyber event occurred or what attack behavior was present**. It does not describe the operational consequence.

Multiple values are permitted when each captures a distinct supported mechanism. A consequence must not be used to infer an attack mechanism, and threat-actor behavior in unrelated incidents must not be used as evidence.

### 7.10. `ImpactTypes`

Multi-value controlled classification of material consequences caused by the incident.

Every final eligible incident requires at least one defensible `ImpactType`. Each selected value needs independent incident-specific support or strong permitted contextual inference.

Attack mechanisms, generic security effects, hypothetical risks, and successfully avoided consequences are not substitutes for material impact.

### 7.11. `AffectedTargetedCompanyDepartments`

Multi-value classification of business functions inside `TargetedCompany` that were materially affected.

The field is function-oriented and does not require the source to use the organization's exact department name. It must not include:

- Departments of another organization.
- Teams that merely participated in remediation.
- Functions presumed to be affected because they normally depend on the compromised technology.

This catalog does not use dynamic `OTHER(...)`. If no catalog value is defensible, use `NO INFORMATION`.

### 7.12. `MostAffectedSupplyChainSection`

Controlled multi-value field representing the supply-chain or value-delivery sections in which material consequences actually manifested.

Despite the singular field name, the stored representation may contain multiple catalog values when multiple sections are independently supported by evidence.

Classification is based on the affected business process itself. It must not be mechanically derived from:

- `AffectedTargetedCompanyDepartments`.
- `AttackTypes`.
- `AffectedSectors`.
- The victim's industry.
- The type of compromised system.

Examples of defensible mappings include halted production to production/manufacturing, impaired checkout to sales/order flow, delayed cargo to distribution/fulfillment or logistics as defined by the catalog, and impaired warehouse processing to warehousing.

This catalog does not use dynamic `OTHER(...)`. A final eligible incident must contain at least one defensible catalog value.

### 7.13. `SupplyChainRelations`

Represents materially affected related organizations and the role performed by `TargetedCompany` relative to each organization.

Canonical form:

`RELATED_COMPANY_NAME(RELATION_VALUE)`

Direction is always:

> `TargetedCompany → materially affected related organization`

The relation answers: **What supply-chain or operational role did the directly attacked organization perform relative to the named affected organization?**

A commercial relationship alone is insufficient. Inclusion requires evidence that:

1. The related organization experienced a material consequence from the same incident.
2. The relationship direction can be established with reasonable confidence.

Customers, suppliers, consultants, investigators, insurers, responders, or partners must not be included merely because they are mentioned in reporting.

### 7.14. `AffectedSectors`

Multi-value controlled classification of economic or industrial sectors that experienced material consequences.

This field is not a simple industry lookup for `TargetedCompany`. A sector may be included when material impact is supported for the targeted organization, a related affected organization, or another clearly documented affected activity.

A service provider's advertised customer portfolio does not establish that every represented sector was affected.

### 7.15. `AffectedCountries`

ISO-3 CSV of countries in which material operational, supply-chain, or value-delivery consequences are supported.

Corporate domicile, headquarters, source publication location, global presence, or customer presence alone do not make a country affected.

### 7.16. `ImpactedGeographicRegions`

Geographic representation of materially impacted regions using the configured `REGION(ISO3)` convention.

The field should describe where incident consequences occurred, not merely where the targeted organization is headquartered or where a source was published. A region-country association requires incident-specific support for material impact in that geography.

### 7.17. `SourceLanguages`

ISO 639-1 CSV describing the languages of the sources used for the incident record. Research-request languages guide discovery; `SourceLanguages` describes the evidence actually retained.

### 7.18. `ThreatActorNames`

Uppercase CSV of source-supported threat-actor names or aliases. Use `NO INFORMATION` when attribution is unavailable or insufficiently grounded.

Threat-actor attribution is not required for TRACE eligibility. An uncorroborated actor claim must not create attack details, impact, or company identity that the evidence does not otherwise support.

### 7.19. `EstimatedSeverity`

Analytical severity estimate from **1 to 100** based only on supported incident consequences.

Relevant considerations include:

- Magnitude of disruption.
- Duration when known.
- Operational scope.
- Capacity or availability loss.
- Number and importance of facilities affected.
- Effects on partners, customers, or dependent organizations.
- Supported financial impact.
- Incident-specific business criticality.

Severity must not be increased merely because a threat actor, malware family, attack type, or victim organization is well known.

The interface may group the numerical score into broader severity classes, but the underlying analytical value remains 1–100.

### 7.20. `NO INFORMATION`

`NO INFORMATION` is the preferred representation for unsupported non-core fields. It is not a failure condition by itself.

TRACE prioritizes defensibility over artificial field completion. Missing non-core information must not be replaced with plausible but unsupported values.

---

## 8. Global catalog interpretation rules

### 8.1. Controlled vocabularies are semantic normalization tools

Catalogs are not keyword dictionaries. A source does not need to reproduce an exact catalog label for that label to be valid.

Equivalent terminology, industry-specific language, functional descriptions, and narrative consequences may be normalized to a controlled value when the mapping is faithful to the evidence.

### 8.2. Evidence is required for every selected value

A classification must not be selected merely because it is:

- Plausible.
- Common for the attack family.
- Typical for the victim's sector.
- Logically possible.
- Associated with another selected field.
- Part of the organization's ordinary business model.

### 8.3. Prefer specificity without false precision

Use the most specific catalog value that the evidence supports. Do not select a narrow category merely because it exists.

### 8.4. Multi-value discipline

For multi-value fields, every additional value must represent a materially distinct supported fact. Neighboring or hierarchical labels must not be stacked solely to increase completeness.

### 8.5. No deterministic cross-field propagation

The following relationships are not automatic:

- `AttackTypes` → `ImpactTypes`.
- `SupplyChainRelations` → `ImpactTypes`.
- `targetType` → `AffectedSectors`.
- `AffectedTargetedCompanyDepartments` → `MostAffectedSupplyChainSection`.
- customer portfolio → `AffectedSectors`.
- service-provider attack → automatic inclusion of every customer as a related affected organization.

Fields must remain semantically consistent but independently supported.

### 8.6. `ANY` and `UNSPECIFIED`

These values are discovery controls only. They never appear in final incident classification.

### 8.7. Dynamic `OTHER(...)`

Where explicitly supported, use:

`OTHER(SCREAMING_SNAKE_CASE_CONCEPT)`

Only when no frozen catalog code can reasonably represent a directly supported concept.

`OTHER(...)` must not be used for synonyms, cosmetic variants, speculative labels, or unnecessary subcategories.

Catalog fallback policy is:

- `AttackTypes`: `OTHER(...)` permitted.
- `ImpactTypes`: `OTHER(...)` permitted.
- `SupplyChainRelations`: `OTHER(...)` permitted.
- `AffectedSectors`: `OTHER(...)` permitted.
- `AffectedTargetedCompanyDepartments`: `OTHER(...)` forbidden.
- `MostAffectedSupplyChainSection`: `OTHER(...)` forbidden.

The Target Type request catalog may use its configured fallback policy, but request-side wildcards remain separate from final incident data.

---

## 9. Cross-field semantic boundaries

### 9.1. Attack mechanism versus consequence

`AttackTypes` describes the cyber mechanism. `ImpactTypes` describes what materially happened to operations or the supply chain. One must not be substituted for the other.

### 9.2. Target organization versus affected organizations

`TargetedCompany` is always the directly attacked entity. Materially affected related organizations are represented through `SupplyChainRelations`.

### 9.3. Target country versus affected countries

`CountryTargetedCompany` identifies the country of the directly attacked entity in the incident. `AffectedCountries` identifies countries where material consequences occurred. These fields may overlap, but they answer different questions.

### 9.4. Department versus supply-chain section

Departments describe internal organizational functions. Supply-chain sections describe where material business-flow consequences manifested. Correlation is allowed, but deterministic mapping is not.

### 9.5. Sector versus organization type

Target organizational type describes what kind of entity is being searched for. `AffectedSectors` describes which economic activities experienced material consequences.

### 9.6. Related organization versus mere business relationship

A documented supplier, customer, platform, or service relationship does not make the counterpart an affected organization. Material same-incident consequence is required.

---

## 10. Reliability Check

### 10.1. Purpose

The Reliability Check is an independent verification pass over an existing TRACE incident. Its purpose is to determine how strongly the incident record is supported by reliable, incident-specific evidence.

The reliability process evaluates the record, it does not rewrite or enrich it. Its output consists of:

- `incidentId`.
- `reliabilityScore` from **0 to 100**.
- `reliabilityFeedback`, a concise explanation of the principal strengths, gaps, contradictions, source-quality problems, or over-inference affecting the score.

### 10.2. Existing records are claims, not evidence

The incident table, its classifications, and its existing source assignments are treated as propositions to verify. Supplied URLs are starting leads rather than the boundary of the investigation.

The check may actively search for stronger corroborating or contradicting evidence.

### 10.3. Verification priorities

Reliability assessment verifies first:

1. Cyber-event identity and the directly attacked `TargetedCompany`.
2. the material TRACE consequence.

It then evaluates:

- `CountryTargetedCompany`.
- `ApproximateIncidentDate`.
- Important attack and impact classifications.
- Affected geographies and related organizations.
- Duration and financial claims when present.
- Source quality.
- Independent corroboration.
- Consistency between sources and fields.
- Correct ordering of `MainSources` and `AdditionalSources`.
- Defensibility of contextual inference.

### 10.4. Reliability scale

| Score      | Interpretation                                                                                                                                                      |
| ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **90–100** | Strong authoritative or direct verification, strong corroboration, and no material unresolved checkable errors.                                                     |
| **85–89**  | TRACE-ready: core predicates, identity, date evidence, and important classifications are supported; limited non-core gaps or strong permitted inference may remain. |
| **75–84**  | Plausible but not TRACE-ready because of meaningful evidence gaps, weak corroboration, identity uncertainty, or unresolved checkable uncertainty.                   |
| **50–74**  | Important evidence or classifications are only partly supported and require substantial review or enrichment.                                                       |
| **25–49**  | Weak or indirect eligibility evidence, poor source quality, material conflicts, or substantial over-inference.                                                      |
| **0–24**   | Incident identity or material consequence is unverified, rumor-driven, contradicted, or lacks credible evidence.                                                    |

A score of **85 or higher** means the evidence is sufficiently reliable for the incident to be documented in TRACE, subject to the platform's review and lifecycle controls.

### 10.5. Inference and reliability

A field must not be penalized solely because it was inferred when the inference is logical, incident-specific, permitted by this specification, and strongly supported by contextual evidence.

Reliability should decrease for unsupported, stretched, contradicted, or generic inference.

### 10.6. Reliability is distinct from eligibility

Both concepts answer different questions:

- **Eligibility:** Does the incident belong in TRACE at all?
- **Reliability:** How defensible is the evidence supporting the record?

---

## 11. Enrichment and information completion

### 11.1. Purpose

Enrichment is a targeted re-investigation of an existing incident to improve evidentiary quality and completeness without broadening into unrelated incidents.

It may operate on a single incident or a batch, with every incident evaluated independently.

### 11.2. Enrichment objectives

Enrichment seeks to:

- Strengthen evidence for the material TRACE consequence.
- Fill fields marked `NO INFORMATION` when better evidence exists.
- Verify weak, uncertain, or inferred values.
- Correct unsupported or misclassified fields.
- Locate stronger primary or independent sources.
- Replace weak evidence where possible.
- Re-rank `MainSources` and `AdditionalSources`.
- Reclassify fields against the complete frozen catalogs.
- Resolve inconsistencies identified during reliability assessment.

### 11.3. Role of `ReliabilityFeedback`

`ReliabilityFeedback` may guide the enrichment search toward weaknesses that need investigation. It is guidance, not evidence.

For example, feedback may direct research toward:

- A questionable date.
- Uncertain target identity
- A weak source bundle
- An unsupported relationship.
- A material consequence supported only by a secondary source.
- An over-inferred classification.

Every correction still requires stronger gathered evidence.

### 11.4. Preservation and correction

Existing values should be preserved only when they remain supported. Enrichment is allowed to correct the record when stronger evidence justifies the change.

It must not invent consequences, relationships, `ImpactTypes`, or supply-chain sections to make the record more complete.

### 11.5. Post-enrichment reliability

After meaningful enrichment, the incident should be eligible for a new reliability assessment so that the score reflects the strengthened or corrected evidence base rather than the earlier record.

---

## 12. Expansion, duplicate control, and incident identity

### 12.1. Expansion purpose

Expansion searches for additional distinct incidents under the same Research Request. It is not an enrichment mechanism for incidents already found.

Existing incidents are treated as exclusion identities during expansion rather than as examples to rewrite, re-source, improve, or paraphrase.

### 12.2. Duplicate-analysis identity

`TargetedCompany` and `CountryTargetedCompany` are the principal normalized identity fields used for duplicate analysis in the research workflow.

`ApproximateIncidentDate` remains important incident evidence but must not serve as the sole duplicate-matching criterion.

Normalization must account for aliases, legal suffixes, parent/subsidiary wording, punctuation, and equivalent company naming without collapsing genuinely distinct attacked entities.

### 12.3. Duplicate and overlap signals

Evidence that candidates may represent the same covered incident includes:

- The same normalized target identity.
- The same target-country identity.
- The same underlying cyber event or causal chain.
- Overlapping core source URLs.
- Titles or descriptions that differ while referring to the same event.

Date wording, publication date, or source phrasing alone must not create a false distinction between duplicates.

### 12.4. Expansion exclusion behavior

An expansion pass should not return an incident already covered by the existing set, including cases in which:

- The title changes.
- The date is phrased differently.
- A parent or subsidiary label changes wording.
- A different source describes the same event.
- A core source URL overlaps with the existing incident.

When better evidence is found for an existing incident, the appropriate operation is enrichment rather than expansion.

---

## 13. Human review and incident lifecycle

Automated research, classification, reliability assessment, and enrichment provide evidence and analytical support. They do not independently determine the final publication state of an incident.

TRACE uses the following incident lifecycle states:

- **`UNDER_REVISION`** — The incident is being researched, verified, enriched, or reviewed.
- **`DISCARDED`** — The incident is about to be completely deleted.
- **`APPROVED`** — The incident has passed internal review for inclusion, not shown to users.
- **`PUBLISHED`** — The incident is shown to general users.

---

## 14. Controlled catalog insertion blocks

The following blocks are reserved for the frozen catalog values. Catalog values should be maintained exactly as defined by the database or authoritative catalog source. Interpretation rules remain governed by Sections 7–9.

### 14.1. Attack Types catalog

**Purpose:** Cybersecurity mechanisms, modalities, or attack archetypes materially present in the incident.

**Fallback policy:** `OTHER(SCREAMING_SNAKE_CASE_CONCEPT)` permitted when no existing code fits a directly supported concept.

**Catalog values:**
- ACCOUNT_TAKEOVER
- ADVERSARY_IN_THE_MIDDLE
- BACKDOOR
- BRUTE_FORCE
- BUSINESS_EMAIL_COMPROMISE
- CREDENTIAL_STUFFING
- CROSS_SITE_REQUEST_FORGERY
- CROSS_SITE_SCRIPTING
- CRYPTOCURRENCY_MINING
- DATA_BREACH
- DATA_DESTRUCTION
- DATA_EXFILTRATION
- DATA_MODIFICATION
- DOS
- DISABLE_SECURITY_CONTROLS
- DDOS
- EXPLOIT_MISCONFIGURATION
- EXPLOIT_VULNERABILITY
- EXTORTION
- HARDWARE_SUPPLY_CHAIN_COMPROMISE
- ICS_OT_COMPROMISE
- IN_MEMORY_MALWARE
- MALWARE
- MALWARE_DOWNLOADER
- PROMPT_BOMBING
- PASS_THE_HASH
- PASSWORD_DUMPING
- PASSWORD_SPRAYING
- PHISHING
- PRETEXTING
- PRIVILEGE_ABUSE
- RANSOMWARE
- REMOTE_ACCESS_TROJAN
- ROOTKIT
- SOCIAL_ENGINEERING
- SOFTWARE_SUPPLY_CHAIN_COMPROMISE
- SOFTWARE_UPDATE_COMPROMISE
- SPEARPHISHING
- SPYWARE_KEYLOGGER
- SQL_INJECTION
- SUPPLY_CHAIN_COMPROMISE
- THIRD_PARTY_PARTNER_COMPROMISE
- TROJAN
- UNAUTHORIZED_ACCESS
- USE_OF_STOLEN_CREDENTIALS
- WEB_APPLICATION_ATTACK
- WIPER
- WORM
- ZERO_DAY_EXPLOITATION

### 14.2. Impact Types catalog

**Purpose:** Material operational, supply-chain, or value-delivery consequences caused by the incident.

**Fallback policy:** `OTHER(SCREAMING_SNAKE_CASE_CONCEPT)` permitted when no existing code fits a directly supported material consequence.

**Catalog values:**
- BACKLOG_OR_ORDER_ACCUMULATION
- CAPACITY_REDUCTION
- DOWNSTREAM_CUSTOMER_DISRUPTION
- DOWNSTREAM_PRODUCTION_INTERRUPTION
- ERP_OR_BUSINESS_SYSTEM_DISRUPTION
- FACILITY_SHUTDOWN
- INVENTORY_OR_DISTRIBUTION_IMPACT
- LOGISTICS_DELAY
- MATERIAL_OR_COMPONENT_SHORTAGE
- OPERATIONAL_DISRUPTION
- ORDER_FULFILLMENT_OR_DELIVERY_DISRUPTION
- ORDER_PROCESSING_DISRUPTION
- OT_OR_ICS_DISRUPTION
- PHYSICAL_DAMAGE
- PLANNING_OR_SCHEDULING_DISRUPTION
- PROCUREMENT_DISRUPTION
- PRODUCT_AVAILABILITY_REDUCTION
- PRODUCT_OR_PROCESS_QUALITY_IMPACT
- PRODUCT_RECALL_OR_WITHDRAWAL
- PRODUCTION_INTERRUPTION
- PRODUCTION_SLOWDOWN
- QUALITY_CONTROL_DISRUPTION
- SAFETY_IMPACT
- SERVICE_DEGRADATION
- SERVICE_UNAVAILABILITY
- SHIPPING_DISRUPTION
- SUPPLIER_DELIVERY_DELAY
- SUPPLIER_OPERATION_DISRUPTION
- SUPPLY_CHAIN_COORDINATION_DISRUPTION
- SUPPLY_CHAIN_DATA_INTEGRITY_IMPACT
- SUPPLY_CHAIN_VISIBILITY_LOSS
- SUPPLY_SHORTAGE
- TRANSPORTATION_DISRUPTION
- WAREHOUSING_DISRUPTION.

### 14.3. Supply Chain Relations catalog

**Purpose:** Role performed by `TargetedCompany` relative to each materially affected related organization.

**Representation:** `RELATED_COMPANY_NAME(RELATION_VALUE)`

**Fallback policy:** `OTHER(SCREAMING_SNAKE_CASE_CONCEPT)` permitted when no existing relation code fits a directly supported relationship.

**Catalog values:**

- BUSINESS_PROCESS_OUTSOURCER
- CLOUD_SERVICE_PROVIDER
- COMPONENT_OR_MATERIAL_SUPPLIER
- CONTRACT_MANUFACTURER
- CONTRACTOR_OR_SUBCONTRACTOR
- CYBERSECURITY_SERVICE_PROVIDER
- DATA_CENTER_OR_HOSTING_PROVIDER
- DIGITAL_PLATFORM_PROVIDER
- DISTRIBUTOR_OR_RESELLER
- DOWNSTREAM_CUSTOMER
- ENGINEERING_OR_MAINTENANCE_SERVICE_PROVIDER
- EQUIPMENT_OR_MACHINERY_SUPPLIER
- HARDWARE_VENDOR_OR_MANUFACTURER
- IT_SERVICE_PROVIDER
- LOGISTICS_INFRASTRUCTURE_OPERATOR
- LOGISTICS_PROVIDER
- MANAGED_SERVICE_PROVIDER
- OT_ICS_SERVICE_PROVIDER
- PAYMENT_OR_FINANCIAL_SERVICE_PROVIDER
- SOFTWARE_VENDOR
- SUBTIER_SUPPLIER
- SYSTEM_INTEGRATOR
- TELECOMMUNICATIONS_PROVIDER
- THIRD_PARTY_SERVICE_PROVIDER
- TRANSPORTATION_PROVIDER
- UPSTREAM_SUPPLIER
- UTILITY_PROVIDER
- WAREHOUSING_PROVIDER.

### 14.4. Affected Sectors catalog

**Purpose:** Economic or industrial sectors that experienced material incident consequences.

**Fallback policy:** `OTHER(SCREAMING_SNAKE_CASE_CONCEPT)` permitted when no existing sector code fits a directly supported affected activity.

**Catalog values:**

- AGRICULTURE_FORESTRY_AND_FISHING
- CHEMICALS
- CONSTRUCTION_AND_ENGINEERING
- EDUCATION_AND_RESEARCH
- ENERGY_AND_UTILITIES
- ENVIRONMENTAL_AND_WASTE_SERVICES
- FINANCE_AND_INSURANCE
- FOOD_AND_BEVERAGE
- FUNERAL_SERVICES
- GOVERNMENT_AND_PUBLIC_ADMINISTRATION
- HEALTHCARE_AND_PUBLIC_HEALTH
- HOSPITALITY_AND_TRAVEL
- LOGISTICS_AND_WAREHOUSING
- MANUFACTURING
- MEDIA_AND_ENTERTAINMENT
- MINING_AND_RAW_MATERIALS
- MUSEUMS_AND_CULTURAL_HERITAGE
- PHARMACEUTICALS_AND_LIFE_SCIENCES
- PHYSICAL_SECURITY_SERVICES
- PROFESSIONAL_AND_BUSINESS_SERVICES
- REAL_ESTATE_AND_COMMERCIAL_FACILITIES
- RETAIL
- TECHNOLOGY_AND_IT_SERVICES
- TELECOMMUNICATIONS
- TRANSPORTATION
- WATER_AND_WASTEWATER
- WHOLESALE_AND_DISTRIBUTION

### 14.5. Affected Targeted Company Departments catalog

**Purpose:** Internal business functions of `TargetedCompany` that were materially affected.

**Fallback policy:** dynamic `OTHER(...)` is not permitted. Use `NO INFORMATION` when no frozen value is defensible.

**Catalog values:**
- ACCOUNTING
- CUSTOMER_SERVICE
- DISTRIBUTION_AND_FULFILLMENT
- ENGINEERING_AND_MAINTENANCE
- FACILITIES_AND_PHYSICAL_OPERATIONS
- FINANCE
- HUMAN_RESOURCES
- INFORMATION_TECHNOLOGY
- INVENTORY_MANAGEMENT
- LEGAL_RISK_AND_COMPLIANCE
- LOGISTICS_AND_TRANSPORTATION
- MARKETING
- ORDER_MANAGEMENT
- PROCUREMENT_AND_SOURCING
- PRODUCTION_AND_MANUFACTURING
- QUALITY_MANAGEMENT
- RETURNS_AND_REVERSE_LOGISTICS
- SALES
- SUPPLY_CHAIN_PLANNING
- WAREHOUSING.

### 14.6. Supply Chain Sections catalog

**Purpose:** Materially affected supply-chain or value-delivery sections stored in `MostAffectedSupplyChainSection`.

**Cardinality:** multi-value CSV; every included section requires independent evidentiary support or strong permitted contextual inference.

**Fallback policy:** dynamic `OTHER(...)` is not permitted.

**Catalog values:**

- DISTRIBUTION&FULFILLMENT
- PROCUREMENT&SOURCING
- PRODUCTION&MANUFACTURING
- WAREHOUSING
- SALES_RETAIL&ECOMMERCE
- SERVICE_DELIVERY