## Building an AgentAPI for Digital Rights

The following steps describe the methodogy I used to build a robust AgentAPI, which is designed to automate common business processes used by consumers to manage their personal vendors.

**STEP ONE**

Envision an open architectural model for processing privacy requests between individuals and organizations worldwide. "Open architecture" means "non-proprietary": it allows users to choose which participating service providers they desire for modular areas of functionality.

**STEP TWO**

Select an existing standard framework as a base guideline for AgentAPI.

I chose the <https://hl7.org/fhir/>FHIR standard as my primary guide. FHIR is an acronym for Fast Health Interoperability Resources, and is designed for the security, integrity, confidentiality, and governance of sensitive personal information such as protected health information. It is robust enough to be implemented over multiple transport protocols, extensible enough to 'future-proof' IT infrastructure investments, structured enough to facilitate accurate functional processing of resources at a granular or composite level, and inclusive enough to accommodate a variety of technical architectures, and parties within and across proprietary domains.
The FHIR 4.0 standard is currently implemented in full-service cloud offerings by Google and Microsoft, which provide a low-cost investment up front to develop and test this Digital Rights implementation immediately.

I also chose Vault API and Control API from VGS (Very Good Security), which provides data protection solutions for PCI-DSS. These APIs provide scenarios for testing interoperability between different systems.

One more scenario involves an emerging API standard, currently under development: the Digital Rights Interface Protocol from Consumer Reports' Digital Lab. This scenario tests 'future-proofing' and interoperability within the AgentAPI.

**STEP THREE**

I conducted an analysis of FHIR Resources and API to explore the many ways in which these artifacts could be used as-is and with modifications or extensions to support Digital Rights interchanges. Originally, the AgentAPI was intended to support privacy rights requests, but has expanded to include other operations which there are no privacy rights legislative mandates, such as "unsubscribing" from email communications, and authorizing agents to purchase products and services on behalf of an individual.   

This analysis included recommendations for developing minimum-effort implementations as well as the most robust implementations to facilitate greater adoption.

**STEP FOUR**

I began building message schemas for privacy access requests, under the CCPA law in the State of California, as well as access requests using the Privacy Act of 1974, and HIPAA under certain provisions of the 21st Century
Cures Act, as it applies to Office of the National Coordinator for Health Information Technology (ONC)'s Health IT Certification Program. The goal was to have a standard schema to support all access requests, including GDPR and mandates from other countries as well.

**STEP FIVE**

I authored this Implementation Guide along with other artifacts contained within this repository.

Modifications made to the FHIR implementation for Digital Rights include:
1) Additional roles such as Student, Applicant, Consumer, Agent, Customer, Employee, FamilyMember, Business, Governmental, Medition, Enforcement, Third-Party, and Not-for-profit organizations, etc.
to augment Person, Patient, Practitioner, Organization, etc. profiles in FHIR.
2) Mapping of some healthcare-specific concepts and classes to privacy rights-specific concepts.

Augmentations to the FHIR implementation include:
1) Integration with VGS Vault API or Anonomatic PII-Vault API
2) Governance with VGS Control API
3) Integration with Consumer Reports Digital Rights Implementation Protocol
