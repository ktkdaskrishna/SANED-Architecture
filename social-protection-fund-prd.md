# Product Requirements Document: Social Protection Fund Analytics and Decision Platform

## Document control
- Product name: Social Protection Fund Analytics and Decision Platform
- Organization: Social Protection Fund, Sultanate of Oman
- Document type: Fully customized Product Requirements Document
- Version: 1.0
- Status: Draft for architecture and delivery planning
- Date: 2026-03-31

## Product overview
The Social Protection Fund Analytics and Decision Platform is a government-grade data, analytics, rules, and AI platform designed to unify beneficiary-relevant data from ministries and approved external entities into a secure, governed environment for eligibility determination, segmentation, policy analytics, and assisted decision-making.[cite:30][cite:32][cite:33]

The platform must support multiple social protection and CSR-style use cases, including student travel aid, food support, newborn support, and future benefit programs, while preserving traceability, access control, and lifecycle management of beneficiaries and households.[cite:28][cite:35][cite:33]

The product is not a replacement for every ministry source system. Instead, it acts as a centralized intelligence and decision layer that integrates cross-sector data, evaluates policy rules, generates recommendations, and exposes governed insights through dashboards, APIs, and Arabic/English conversational interfaces.[cite:32][cite:36][cite:39]

## Problem statement
Social protection data is fragmented across ministries, civil systems, operational databases, and service providers, which makes it difficult to identify vulnerable households consistently, assess eligibility quickly, and monitor benefit delivery across programs.[cite:32][cite:36][cite:39]

Without an integrated platform, the Fund risks duplicated records, inconsistent eligibility decisions, delayed identification of newly eligible households, and limited visibility into the full impact of social assistance programs.[cite:32][cite:33]

The intended platform addresses this by establishing a governed integrated registry, configurable eligibility logic, analytics-ready data models, ML-assisted segmentation, and a controlled AI assistant that can answer operational and policy questions in natural language.[cite:32][cite:33][cite:37]

## Vision and objectives
The platform vision is to create a single trusted environment where the Social Protection Fund and authorized partners can understand who needs support, why they qualify, which intervention is appropriate, and how assistance should be monitored over time.[cite:32][cite:39]

Primary objectives are:
- Build a secure integrated person, household, and institution registry across approved source systems.[cite:32][cite:33]
- Enable policy administrators to define, version, and execute eligibility and prioritization rules without code releases.[cite:33][cite:42]
- Support analytics at national scale for millions of residents, households, institutions, and transactions across multiple benefit programs.[cite:32][cite:39]
- Provide AI-assisted access in Arabic and English for authorized users to query eligibility, segments, program coverage, and funding allocation scenarios.[cite:37][cite:34]
- Maintain full auditability, security, data residency alignment, and controlled access to sensitive personal information.[cite:30][cite:37]

## Product scope
### In scope
- Cross-ministry and cross-entity data ingestion.
- Raw data lake and curated analytics lakehouse layers.
- Data quality, deduplication, entity resolution, and household resolution.
- Eligibility rules engine and segment builder.
- ML pipelines for clustering, ranking, recommendation, anomaly detection, and policy simulation.
- Operational dashboards and analytical reporting.
- Arabic/English AI assistant and agent builder for governed workflows.
- Security controls, IAM, key management, encryption, audit logging, and monitoring.
- Controlled CSR allocation workflows and recommendation outputs for external approved contributors.

### Out of scope
- Replacing source ministry operational systems.
- Core payment execution rails unless integrated via external finance systems.
- Citizen-facing appeals management in full depth for phase 1, except status exposure and case routing hooks.
- Open-ended generative AI with unrestricted access to raw data.

## Stakeholders
### Executive stakeholders
- Social Protection Fund leadership.
- Ministry data owners and policy owners.
- National digital government and security governance stakeholders.

### Operational stakeholders
- Policy administrators.
- Program administrators.
- Data governance team.
- Data engineering and ML operations teams.
- Security and compliance teams.
- Analysts and executive reporting users.
- Approved CSR organizations and institutional donors.

## Users and personas
| Persona | Description | Primary needs |
|---|---|---|
| Platform Admin | Manages platform configuration, tenancy, access, keys, integrations, and controls | Secure configuration, observability, auditability |
| Policy Admin | Defines eligibility criteria and benefit logic | No-code rules, versioning, simulation, approval workflows |
| Data Analyst | Explores data and produces reports | Trusted curated datasets, lineage, self-service query access |
| Program Operator | Reviews beneficiary recommendations and actions | Explainable eligibility, status, workflow queues |
| Executive User | Consumes strategic insights | Dashboards, trends, scenario analysis |
| CSR Manager | Allocates institutional or philanthropic funds | Need-based recommendations, allocation constraints, justification |
| Auditor/Compliance Officer | Reviews access and decisions | Immutable logs, traceability, policy history |

## Core use cases
### Use case 1: Student travel aid
A policy administrator configures rules for monthly student travel aid of 50 OMR for eligible students based on residence, institution status, enrollment verification, household context, and any location constraints. The platform ingests education and residency data, resolves the individual to a household, evaluates current eligibility, and produces a beneficiary list with a decision explanation and payment-ready export to downstream systems.[cite:32][cite:33][cite:36]

### Use case 2: Newborn support
When a birth event is received from an approved health or civil source, the platform links the newborn to parents or household records, checks the support rule for newborn assistance, applies any nationality or residency requirements, and flags the household for the approved newborn support amount. The system records the event source, benefit justification, and whether the case was auto-approved or sent for manual review.[cite:32][cite:36][cite:39]

### Use case 3: CSR allocation
A CSR manager enters a budget such as 10,000 OMR and asks in Arabic which people, households, hospitals, or partner institutions should be prioritized. The AI assistant calls governed tools to query approved segments, scoring models, unmet need indicators, and institutional capacity data, then proposes ranked options with justification, budget fit, geography, and policy-safe constraints.[cite:34][cite:37][cite:33]

## Business requirements
- The platform must maintain a unified and current registry of persons, households, benefits, and institutional entities relevant to social protection operations.[cite:32][cite:33]
- The platform must support multiple protection programs and allow new programs to be added without redesigning the core data architecture.[cite:39][cite:42]
- The platform must reduce the time needed to identify eligible households and generate approved beneficiary lists.[cite:32]
- The platform must improve fairness and consistency by evaluating the same policy rules against governed and versioned data assets.[cite:33][cite:42]
- The platform must provide explainable outcomes for decisions, segments, and recommendations.[cite:33][cite:37]

## Functional requirements
### FR-1 Data source onboarding
The platform must ingest data from APIs, file drops, database replication, message queues, and approved manual uploads from ministries and partner entities. Each source onboarding workflow must capture source ownership, schema, refresh cadence, data sensitivity, quality expectations, and legal sharing basis.[cite:32][cite:36]

### FR-2 Data lake and storage zones
The platform must maintain at least three storage zones: raw landing, standardized processing, and curated analytics-serving layers. Raw data must be preserved unchanged for forensic traceability, while standardized and curated layers must support governed downstream use.[cite:32][cite:39]

### FR-3 Data quality
The platform must validate schema conformity, mandatory fields, referential integrity, uniqueness rules, freshness SLAs, and business plausibility checks. Records that fail validation must be quarantined or flagged with remediation workflows rather than silently accepted.[cite:32][cite:33]

### FR-4 Identity and household resolution
The platform must resolve person identity across source systems using deterministic and probabilistic matching where approved. It must support household construction and updates based on family relationships, residence, dependency, and event changes.[cite:32][cite:36][cite:39]

### FR-5 Integrated registry
The platform must maintain curated entities for person, household, institution, benefit program, event, decision, payment reference, geography, and supporting social indicators. The registry must be historized so users can determine what the record looked like at a point in time.[cite:32][cite:33]

### FR-6 Eligibility engine
Policy administrators must be able to define rules using fields such as age, education status, income proxy, household size, disability indicators, newborn events, region, employment status, and benefit history. Rules must support thresholds, Boolean logic, lookback windows, effective dates, overrides, simulation, and approval workflows before production use.[cite:33][cite:42]

### FR-7 Segmentation and tagging
Authorized users must be able to create reusable segments and tags such as students, low-income households, elderly over 80, elderly over 50, newborn households, unemployed households, or high-risk geographic clusters. Segments must be queryable through dashboards, APIs, and the AI assistant.[cite:32][cite:39]

### FR-8 Machine learning
The platform must support ML workflows for clustering, risk scoring, prioritization, anomaly detection, and recommendation. Model outputs must be versioned, monitored, explainable, and reviewable before being used in operational recommendations.[cite:34][cite:37]

### FR-9 Dashboards and analytics
The platform must provide dashboards for enrollment, active beneficiaries, regional distribution, coverage gaps, budget utilization, program overlap, and benefit outcomes. Users must be able to filter by time, geography, program, household segment, and demographic attributes according to authorization.[cite:32][cite:33]

### FR-10 Conversational AI
The platform must provide an Arabic and English conversational interface that lets authorized users ask questions such as which people are eligible, what segments exist, what gaps remain, and how to allocate a CSR budget. The assistant must answer only from governed tools and datasets, and every response must carry decision traceability and permission checks.[cite:37][cite:34]

### FR-11 Agent builder
Administrators must be able to configure AI agents, prompts, tools, policies, guardrails, retrieval sources, and workflow actions. Agents must support approved tasks such as eligibility lookup, funding recommendation, hospital support analysis, and segment comparison, with human approval steps where required.[cite:37]

### FR-12 Audit and traceability
Every material action, including source ingestion, schema changes, rule changes, model promotion, user access, AI prompt execution, query execution, and recommendation generation, must be logged with time, actor, justification, and outcome.[cite:33][cite:37]

## Non-functional requirements
### NFR-1 Scale
The platform must support national-scale analytics for approximately 4.5 million people and associated households, institutions, and event histories, with horizontal scalability for large joins, reprocessing, and model execution.

### NFR-2 Performance
- Standard dashboard interactions should return within 5 seconds for common filters.
- Common eligibility lookups through curated datasets should return within 10 seconds.
- Batch eligibility runs for scheduled programs should complete within agreed processing windows.
- AI assistant responses for governed analytical prompts should typically return within 15 seconds, excluding long-running workflows.

### NFR-3 Availability
- Platform target uptime: 99.9% for core analytical services.
- Recovery point objective: 15 minutes for critical metadata and workflow state.
- Recovery time objective: 4 hours for major service disruption.

### NFR-4 Security
- Encryption in transit must be enforced across all network paths.
- Encryption at rest must be enabled for all storage tiers, metadata stores, and backups.
- Customer-managed or organization-managed key controls must be supported where mandated.
- Sensitive data elements must support masking, tokenization, or field-level access restrictions.

### NFR-5 Compliance and governance
The platform must support data residency requirements, segregation of duties, audit retention, access reviews, key rotation, data retention policies, and consent or sharing constraints as applicable to the legal framework and operating agreements.[cite:30][cite:37]

### NFR-6 Explainability
Operational recommendations must provide enough context for a reviewer to understand why a household or institution was included, excluded, ranked, or flagged.

## Data requirements
### Source domains
Expected source domains include:
- Civil identity and family records.
- Birth and death events.
- Education enrollment and attendance.
- Employment and wage-related information.
- Health and disability indicators where legally approved.
- Address and geography records.
- Existing benefit and payment history.
- Institutional capacity records for hospitals or approved service providers.
- CSR contributions, allocation pools, and sponsor constraints.

### Core entities
- Person.
- Household.
- Relationship.
- Address.
- Student status.
- Employment status.
- Income indicator.
- Benefit program.
- Eligibility decision.
- Payment or referral record.
- Institution.
- Capacity indicator.
- Life event.
- Geographic hierarchy.
- Audit event.

### Data governance metadata
For every dataset, the platform must capture data owner, steward, schema version, classification, refresh frequency, SLA, lineage path, retention policy, and allowed use cases.[cite:32][cite:33]

## Product workflow
### End-to-end workflow
1. Source systems send data through approved integration channels.
2. Raw data lands in a protected ingestion zone with source-level isolation.
3. Validation, standardization, and entity resolution pipelines process incoming records.
4. Curated registry and subject-area models are updated.
5. Eligibility rules and model pipelines execute on schedule or on demand.
6. Outputs are published to analytics marts, dashboards, APIs, and governed AI tools.
7. Program operators review, approve, export, or route actions.
8. All actions and outcomes are logged for audit and policy traceability.

### Decision workflow
1. A user requests a segment, beneficiary list, or CSR allocation recommendation.
2. The system checks role, permission, and data-scope entitlements.
3. The query is translated into approved analytical actions.
4. The system retrieves governed data and rules outputs.
5. ML or ranking logic is invoked where the use case permits.
6. A response is generated with explanation, confidence or ranking basis, and actionable outputs.
7. High-impact actions are routed for approval before any downstream execution.

## Detailed architecture requirements
### Platform pattern
The recommended pattern is a secure lakehouse architecture rather than a warehouse-only architecture, because the solution must support raw multi-format ingestion, large-scale transformations, iterative ML, feature reuse, and curated serving layers without forcing all data into a traditional warehouse shape at the start. Adaptive social protection systems also benefit from integrated registries and data flows that combine operational and analytical perspectives.[cite:32][cite:39]

### Logical layers
- Source integration layer.
- Landing and data lake storage layer.
- Processing and transformation layer.
- Master and integrated registry layer.
- Curated analytics and serving layer.
- Rules and ML layer.
- API and conversational intelligence layer.
- Security, audit, and observability layer.

### Preferred technology direction
A modern approach would use cloud object storage as the raw and standardized data lake, a distributed lakehouse engine for ETL and ML, a governed SQL serving layer for curated analytics, and an AI orchestration layer for the conversational and agentic experience. This direction aligns with the need for large-scale processing, model operations, and flexible analytics rather than relying on log analytics tooling as the primary core.[cite:34][cite:37]

### Tooling guidance
Splunk is not the preferred primary platform for this project because the dominant workload is integrated analytics, data engineering, eligibility evaluation, and ML-assisted recommendation rather than security log analytics and operational event monitoring. Splunk may still be used as an observability or audit consumer if needed, but not as the core social protection analytics platform.[cite:34]

## Security architecture requirements
### Identity and access
- Integrate with centralized IAM and SSO.
- Enforce RBAC and, where needed, ABAC based on role, department, geography, program, and data sensitivity.
- Support service identities for pipelines, agents, and integrations.
- Enforce just-in-time elevated privileges for administrative actions where possible.

### Encryption and key management
- Support customer-managed keys or organization-controlled key management for critical stores.
- Separate encryption domains for raw data, curated data, secrets, and model artifacts where possible.
- Rotate keys according to policy and log all key usage and administrative changes.

### Data protection
- Mask or tokenize national identifiers and other high-sensitivity fields in lower-trust contexts.
- Apply row-level and column-level controls to sensitive datasets.
- Protect prompts, model outputs, and audit trails from unauthorized access.

### AI guardrails
- Prevent unrestricted model access to raw sensitive data.
- Restrict tool use to approved connectors and governed query templates.
- Log prompts, tool calls, outputs, and approvals for accountability.[cite:37]

## Integration requirements
### Upstream integrations
- Ministry systems.
- Health and civil event systems.
- Education systems.
- Labor and income-relevant systems.
- Geographic or municipal systems.
- Institutional capacity systems.
- Payment or finance systems where applicable.

### Downstream integrations
- Dashboards and BI tools.
- Notification or workflow tools.
- External payment systems.
- Audit and SIEM platforms.
- Case management tools.
- AI model endpoints and orchestration services.

### Integration standards
- Support secure APIs, batch file ingestion, database connectors, and event-driven interfaces.
- All interfaces must support authentication, schema contracts, monitoring, and replay or retry controls.

## Arabic and multilingual requirements
- The UI must support Arabic-first and English-first operation.
- The AI assistant must understand Arabic queries written in formal or common user phrasing.
- Key entities, segments, and policy labels must support bilingual metadata.
- Explanations shown to end users must preserve meaning across languages and not alter policy interpretation.[cite:37]

## Analytics and ML requirements
### Analytical domains
- Eligibility monitoring.
- Coverage analysis.
- Geographic need mapping.
- Budget utilization and forecast.
- Program overlap analysis.
- Demographic segmentation.
- Institutional capacity planning.
- CSR impact allocation.

### ML domains
- Household similarity clustering.
- Prioritization and ranking.
- Poverty or vulnerability proxy modeling where legally approved.
- Benefit leakage and anomaly detection.
- Early warning signals for unmet needs.

### ML governance
- Training data, feature definitions, model versions, evaluation metrics, and approval status must be recorded.
- Models used in operational recommendations must be explainable to authorized reviewers.
- High-risk models must require governance approval before release.[cite:37]

## UX requirements
### Admin console
The platform must provide an admin console for source onboarding, schema review, rule configuration, agent configuration, model registry visibility, and access policy administration.

### Analyst workspace
The platform must provide curated datasets, saved segments, lineage visibility, scheduled reporting, and notebook or SQL-compatible analytical access for authorized teams.

### Operational workspace
The operational workspace must show pending eligibility runs, review queues, justification details, and export or routing options.

### Conversational workspace
The AI workspace must support chat, explanation panels, suggested prompts, downloadable outputs, approval requests, and agent-triggered workflows for approved tasks.

## Example user journeys
### Journey 1: Monthly student support
1. A scheduled workflow receives updated education enrollment and residence data.
2. Identity resolution links students to households.
3. The student travel rule runs.
4. A beneficiary list is created with reasons and exceptions.
5. Program staff review flagged cases.
6. An approved export is sent to downstream financial processing.

### Journey 2: Birth-triggered support
1. A birth event enters the system.
2. The event is matched to guardians and household.
3. The newborn support rule runs automatically.
4. If confidence and policy checks pass, the case is queued for approval or auto-action.
5. The decision and source evidence are stored for audit.

### Journey 3: CSR allocation by Arabic chat
1. A CSR manager types in Arabic asking how to spend 10,000 OMR.
2. The assistant checks entitlements and policy boundaries.
3. The assistant evaluates approved segments, unmet need, and capacity constraints.
4. Ranked options are returned, for example households, hospitals, or mixed allocation scenarios.
5. The manager submits a recommendation package for approval.

## Reporting requirements
Required reporting views include:
- Eligible versus served population by program.
- Regional distribution of need and benefits.
- Aid utilization over time.
- Segment trends by age, household type, student status, and newborn events.
- Program overlap and duplication risk.
- CSR allocation outcomes.
- Audit and operational performance metrics.

## Success metrics
### Operational metrics
- Time to onboard a new source system.
- Time to produce a beneficiary list after source refresh.
- Percentage of records passing quality rules on first processing.
- Reduction in duplicate beneficiary records.

### Decision quality metrics
- Eligibility precision and false-positive review rate.
- Percentage of decisions with machine-readable explanation.
- Model stability and drift indicators.

### User adoption metrics
- Monthly active analyst and operator users.
- Number of approved policy rules managed without code release.
- Number of conversational queries completed successfully in Arabic and English.

### Security and governance metrics
- Access review completion rates.
- Mean time to detect and respond to abnormal access or pipeline failures.
- Percentage of high-sensitivity datasets protected by fine-grained controls.

## Risks and mitigations
| Risk | Impact | Mitigation |
|---|---|---|
| Poor source data quality | Incorrect or delayed decisions | Quarantine flows, stewardship workflow, source SLAs |
| Inconsistent identifiers across ministries | Duplicate or fragmented households | Entity resolution framework and golden record approach |
| Overexposure of PII via AI or analytics | Compliance and trust failure | Fine-grained controls, masking, governed tools, logging |
| Model bias or opaque recommendations | Unfair allocation or rejected trust | Explainability, review gates, governance approval |
| Integration delays with ministries | Program rollout delays | Phased onboarding and canonical source contracts |
| Overcomplicated phase 1 scope | Delivery slippage | Prioritize high-value programs and staged releases |

## Release roadmap
### Phase 1: Foundation
- Secure cloud landing zone.
- Core data lakehouse.
- Initial source onboarding.
- Person and household registry.
- Basic dashboards.
- Foundational IAM, encryption, and audit.

### Phase 2: Policy automation
- Rules engine.
- Segment builder.
- Admin console.
- Benefit-specific workflows for student and newborn support.
- Explainability layer.

### Phase 3: Advanced intelligence
- ML pipelines.
- Recommendation engine.
- CSR allocation support.
- Arabic/English conversational assistant.
- Agent builder.

### Phase 4: Ecosystem maturity
- Broader ministry integrations.
- Expanded institutional support use cases.
- Advanced forecasting and scenario planning.
- More automated downstream action orchestration.

## Open questions
- Which datasets are legally approved for household linkage and vulnerability scoring?
- Which use cases require near-real-time processing versus daily or monthly batch?
- What are the authoritative source systems for identity, household membership, and address?
- Which fields can be exposed in CSR scenarios versus only used internally for ranking?
- What approval thresholds are required before AI-generated recommendations can produce operational outputs?

## Acceptance criteria for PRD approval
This PRD will be considered ready for solution design when:
- Stakeholders agree on in-scope programs for phase 1.
- Source owners and legal data-sharing basis are identified.
- Security and key-management principles are approved.
- Eligibility, segmentation, and AI-assistant operating boundaries are accepted.
- Scale, performance, and roadmap assumptions are accepted by engineering and operations.
