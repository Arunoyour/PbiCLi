# AI & Data Regulations — A Pre-Build Reference

A checklist of the major global regulations to choose from *before* building an AI agent or PDLC (Product Development Life Cycle) solution — not after it's already handling real user data.

## Why This Exists

Anyone can build an AI agent today with very little friction. What's easy to skip is asking: *once this agent is live, what stops it from quietly collecting, exposing, or selling the data it touches?* Most PDLC tooling has no built-in regulatory guardrail — it will do whatever it's told, including things that are illegal in the jurisdiction the user operates in.

This file is a reference list of real, currently active regulations around the world that govern data privacy, AI-specific behavior, or both. The intent: before implementation starts, the builder picks which of these apply to their users, their data, and their region — and designs the agent around those constraints from day one, not as a retrofit.

**Note:** Laws change and enforcement details vary by case. This file is a starting map for picking the right regulations to dig into, not a substitute for legal counsel.

## How to Use This File

1. Identify **where your users are located** (regulations are usually triggered by the user's location, not the company's).
2. Identify **what kind of data** the agent touches (personal data, health data, financial data, biometric data, children's data — each can trigger its own regulation).
3. Identify **what the AI itself does** (does it make automated decisions about people? Profile them? Generate content? Act autonomously?) — this is what AI-specific regulations target, separately from general data-privacy law.
4. Cross-check against the lists below and mark which ones apply.
5. Build the compliance constraints (consent, data retention limits, deletion rights, audit logs, human review for high-risk decisions) into the agent's design *before* writing the implementation — not as a patch afterward.

## General Data Privacy Regulations (by Region)

| Regulation | Region | Small Description | Example Trigger |
|---|---|---|---|
| **GDPR** (General Data Protection Regulation) | European Union / EEA | Governs how personal data of EU residents is collected, stored, processed, and shared; requires a lawful basis for processing, and gives people rights to access, correct, and delete their data. | An agent stores an EU user's email and chat history without a clear legal basis or deletion path — this is a GDPR violation regardless of where the company is based. |
| **UK GDPR + Data Protection Act** | United Kingdom | The UK's post-Brexit equivalent of GDPR, closely aligned but enforced separately by the UK's ICO. | A UK-only agent still needs its own UK GDPR compliance even if it already follows EU GDPR. |
| **CCPA / CPRA** (California Consumer Privacy Act / Privacy Rights Act) | California, USA | Gives California residents the right to know what personal data is collected, opt out of its sale, and request deletion. | An agent that silently feeds user conversation data to a third-party ad network triggers CCPA's "sale/sharing of data" obligations. |
| **Other US state laws** (Virginia CDPA, Colorado CPA, Connecticut CTDPA, and a growing list of others) | USA (state-by-state) | Similar in spirit to CCPA but with state-specific thresholds and rights — there is no single US federal privacy law. | An agent serving users across multiple US states may need to honor different opt-out and deletion rules depending on which state the user is in. |
| **PIPEDA** (Personal Information Protection and Electronic Documents Act) | Canada | Requires consent for collecting personal data and reasonable safeguards for how it's used and stored. | An agent built for a Canadian user base needs explicit consent language before storing identifying data. |
| **LGPD** (Lei Geral de Proteção de Dados) | Brazil | Brazil's GDPR-equivalent; governs consent, data subject rights, and cross-border data transfer. | A Brazilian user's data moved to a server outside Brazil without an approved transfer mechanism can breach LGPD. |
| **PDPA** (Personal Data Protection Act) | Singapore (also similarly named laws in Thailand, Malaysia, etc.) | Regulates collection, use, and disclosure of personal data, with required consent and data-breach notification. | An agent operating in Singapore must notify affected users and the regulator within a set window after a data breach. |
| **POPIA** (Protection of Personal Information Act) | South Africa | Sets conditions for lawful processing of personal information, similar in structure to GDPR. | Processing a South African user's data without meeting one of POPIA's lawful-processing conditions is non-compliant. |
| **DPDP Act** (Digital Personal Data Protection Act) | India | Requires clear consent for processing personal data, with specific obligations around children's data and cross-border transfer. | An agent targeting Indian users must get verifiable consent before processing data of a minor. |
| **APPI** (Act on the Protection of Personal Information) | Japan | Regulates how businesses handle personal information, including cross-border transfer restrictions. | Transferring a Japanese user's data to a country without adequate protections may require extra safeguards under APPI. |
| **PIPL** (Personal Information Protection Law) | China | China's comprehensive data-privacy law, with strict rules on cross-border data transfer and processing of "sensitive" personal information. | An agent handling Chinese users' data may be legally required to store it within China or go through a formal cross-border transfer approval. |

## AI-Specific Regulations and Frameworks

These go beyond "data privacy" and target the *behavior of the AI system itself* — automated decision-making, risk classification, transparency, and accountability.

| Regulation / Framework | Region | Small Description | Example Trigger |
|---|---|---|---|
| **EU AI Act** | European Union | Classifies AI systems by risk level (unacceptable, high, limited, minimal) and imposes obligations scaled to that risk — including transparency, human oversight, and documentation for high-risk systems. | An agent used to screen job applicants or score creditworthiness is likely "high-risk" under the EU AI Act and requires documented risk management, human oversight, and transparency to affected individuals. |
| **NIST AI Risk Management Framework (AI RMF)** | United States (voluntary framework, widely referenced) | A structured, non-binding framework for identifying, measuring, and managing risks in AI systems across their lifecycle. | A team building an autonomous agent uses the AI RMF's "Govern/Map/Measure/Manage" structure to define what gets logged, tested, and reviewed before launch. |
| **China's Generative AI Measures** | China | Regulates generative AI services specifically — requiring security assessments, content labeling, and alignment with state content rules. | An AI content-generation agent offered to users in China must comply with mandatory content review and labeling requirements. |
| **Canada's AIDA** (Artificial Intelligence and Data Act, proposed) | Canada | Proposed framework (still evolving) to regulate "high-impact" AI systems with obligations around risk assessment and mitigation. | A Canadian company deploying an agent that makes automated employment decisions would need to track this law's development closely. |
| **ISO/IEC 42001** | International (voluntary standard) | The first international standard for an AI management system — a certifiable framework for responsibly building, operating, and improving AI systems. | An enterprise adopting ISO/IEC 42001 builds agent governance (risk assessment, monitoring, incident response) into its PDLC process as a certifiable discipline, not an afterthought. |

## Sector-Specific Regulations (Apply on Top of the Above)

| Regulation | Sector | Small Description | Example Trigger |
|---|---|---|---|
| **HIPAA** | Healthcare (USA) | Protects health information; requires specific safeguards for storing, transmitting, and sharing patient data. | An agent summarizing patient records must not expose that data to a model or logging pipeline that isn't HIPAA-compliant. |
| **GLBA** (Gramm-Leach-Bliley Act) | Financial services (USA) | Requires financial institutions to explain information-sharing practices and safeguard sensitive data. | An agent handling a user's bank transaction history for a budgeting tool falls under GLBA's safeguarding requirements. |
| **COPPA** (Children's Online Privacy Protection Act) | Services directed at children (USA) | Requires verifiable parental consent before collecting personal data from children under 13. | An AI tutoring agent for kids must not collect a child's voice recordings or chat logs without parental consent. |
| **PCI DSS** | Payment card data (global industry standard) | Sets security requirements for any system that stores, processes, or transmits credit card data. | An agent that handles checkout or payment details must meet PCI DSS controls even though it's not a government law. |

## A Concrete Example

**The scenario:** A team builds an AI agent to automatically screen job applications and recommend candidates, planning to sell it to companies across the US, EU, and India.

**What's missed without this checklist:** The team builds the agent purely on functionality — accuracy of candidate scoring — and ships it. No one checks whether automated candidate scoring counts as a "high-risk" AI use case.

**What should have happened:** Before implementation, the team identifies that:
- EU users trigger the **EU AI Act**'s high-risk obligations (human oversight, documentation, transparency to applicants).
- Indian users trigger the **DPDP Act**'s consent requirements for personal data processing.
- US users may trigger **state-level anti-discrimination and automated-decision disclosure laws**, depending on the state.

The agent is then designed with a human-in-the-loop review step for rejections, a documented risk assessment, and region-aware consent flows — built in from the start, instead of being retrofitted after a regulator or a rejected applicant raises the issue.

## Key Takeaway

An AI agent built on a PDLC solution has no regulatory awareness by default — it will do whatever it's instructed to do with whatever data it's given. The regulations above aren't a formality to check off after launch; they are the actual boundary of what the agent is legally allowed to do with people's data. Picking the applicable ones *before* building determines the agent's design — consent flows, data retention, human oversight, and what it's never allowed to do — rather than being a cleanup task after something goes wrong.

---

Sources for further reading:
- [Global Data Privacy Regulations 2026 — Country-by-Country Guide](https://gdprlocal.com/global-data-privacy-regulations/)
- [Privacy Laws 2026: Global Updates & Compliance Guide](https://secureprivacy.ai/blog/privacy-laws-2026)
- [2026 Guide to Global Data Privacy Regulations & Compliance](https://accutivesecurity.com/data-privacy-regulations-from-around-the-world-a-comprehensive-guide/)
- [Data Privacy Laws 2026: International Guide to 50+ Jurisdictions](https://globallawlists.org/insights/international-lawyers-guide-data-privacy-laws-2026-navigating-50-plus-jurisdictions)

Regulatory text and enforcement details change frequently — verify current requirements with legal counsel or the regulator's own published text before relying on this for a real build.
