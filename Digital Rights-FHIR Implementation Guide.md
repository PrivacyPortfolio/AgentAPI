## FHIR IMPLEMENTATION GUIDELINE FOR DIGITAL RIGHTS 

===================================================
### Topic: Architectual Overview

**FHIR implementation**

At its core, FHIR contains two primary components:

Resources - a collection of information models that define the data elements, constraints and relationships 
for the business objects most relevant to healthcare. From a model-driven architecture perspective, 
FHIR resources are notionally equivalent to a physical model implemented in XML or JSON.

APIs - a collection of well-defined interfaces for interoperating between two applications. 
Although not required, the FHIR specification targets RESTful interfaces for API implementation. 

In the healthcare domain, there is a notional and ongoing evolutionary, consensus-based process 
for standardizing on a core set of common business objects including things like: a patient, a procedure, an observation, an order, etc. (see a list of defined resources). 
The FHIR specification provides a framework for defining these healthcare business objects (resources), 
for relating them together in a compositional manner, for implementing them in a computable form, 
and for sharing them across well-defined interfaces. 
The framework contains a verifiable and testable syntax, a set of rules and constraints, 
methods and interface signatures for FHIR-aware APIs, and specifications for the implementation of a server 
capable of requesting and delivering FHIR business objects.

**Digital Rights implementation**

FHIR architecture's separation of content (resources) from process (APIs) makes possible adapting this framework for digital rights processes.
Process is not limited to the restful API: FHIR supports a wide variety of transport protocols in addition to HTTP, and its resources are designed to support without hard dependencies, a wide variety of implementation and storage options.
Likewise, Digital Rights can be composed as structured Requests and Responses which represent 
"a data subject", "a privacy right", "a vulnerability", and "a remediation".

The minimum resources for a digital rights privacy process are: 
2 parties: the data subject, and the business.
2 messages: the privacy right as "request", and the fulfillment as "response".
2 systems: the conceptual proxy for a source (client), and a target (server).

============================
### Topic: Specification Overview

**FHIR implementation**

The base FHIR specification (this specification) describes a set of base resources, frameworks and APIs that are used in healthcare. 
Due to wide variability between jurisdictions and across the healthcare ecosystem around practices, requirements, regulations, education, not every action or resource is feasible and/or beneficial.

The FHIR specification, as a "platform specification", has particular contexts of use to support a variety of different adaptations. Typically, these adaptations specify:
- Rules about which resource elements are or are not used, and what additional elements are added that are not part of the base specification.
- Rules about which API features are used, and how they are used.
- Rules about which terminologies are used in particular elements.
- Descriptions of how the Resource elements and API features map to local requirements and/or implementations.

Because of the nature of the healthcare ecosystem, there may be multiple overlapping sets of adaptations - 
by healthcare domain, by country, by institution, and/or by vendor/implementation.

FHIR defines a cascade of artifacts for this purpose:
1) Implementation Guide (IG): A coherent and bounded set of adaptations that are published as a single unit.
            This Guide is an example, although the Digital Rights adaptation is published adjacent to the FHIR Core specification for reference purposes.
2) Package:	A group of related adaptations that are published as a group within an Implementation Guide	
						US Core Capability Statements is an example of a Package. 
3) Conformance Resource: A single resource in a package that makes rules about how an implementation works. 
						DAF Problem Value Set is an example of a Conformance Resource. It is broadly defined as a series of brief statements that catalog a patient's signs, symptoms, and defined conditions that are relevant to that patient's healthcare.
4) Profile: A set of constraints on a resource represented as a structure definition with kind = constraint	
						DAF Medication Request is an example of a Profile that defines constraints and extensions on the MedicationRequest resource for the minimal set of data to query and retrieve prescription information.

**Digital Rights implementation**

Privacy laws and procedures, like healthcare, are also widely vary in content and process around the world.
Even if this were not the case, each party's implementation of a digital rights specification would differ:
not every party has the same technical capabilities, nor do they all support the same resources.

Fortunately for us, the FHIR architecture supports customized adaptations. For example:
THE FHIR specification accommodates these differences through artifacts such as a Server Capability Statement, and versioning of content resources and the FHIR framework itself.

Instead of a DAF Problem Value Set for healthcare adaptations, a Digital Rights Vulnerability Value Set could be a Conformance Resource that describes potential harms. Another Conformance Resource could be a Conformance/Violation List which catalogs a set of digital rights statues and regulations. Yet another Conformance Resource could be a list of remediation requirements. 

A Profile is used as a set of constraints on a resource such a Person, Organization, or Practitioner.
Person and Organization are nouns representing two different things. A Practitioner is an adjective used to describe a Resource, such as a Person or an Organization. Profiles are well-suited to define the contextual roles of these resources, and the constraints and extensions on their structure and use. A Profile could also govern a selected resource such as a RemediationRequest, represented as a structure definition with kind = constraint.	

============================
### Topic: Framework Overview

**FHIR implementation**

Foundation Resources:	Foundation resources are the most rudimentary, foundational resources. They are often used for infrastructural tasks. Although not prohibited, they are not always referenced by other resources.

Base Resources:	Layer 2 consists of base resources. These are often the leaf nodes of a resource graph. In other words, they are often referenced by other resources, but don't typically reference other resources themselves. 

Clinical Resources:	Layer 3 includes the resources that are clinical in nature but are also very common across many use cases.	This includes resources for clinical observations, clinical treatment, care provision, and medications. 

Financial Resources: Layer 4 is dedicated to financial resources. Logically, financial resources build on clinical and base resources. For example, a billing resource will reference clinical events and activities as well as base resources like a patient.

Specialized Resources: In layer 5, we find more specialized resources for less common use cases. These resources almost always reference resources	in lower layers.

Resource Contextualization: Layer 6 does not contain resources. However, it does extend the composition framework made up by the first five layers of resources. Layer 6 includes profiles and graphs. 

**Digital Rights implementation**

Foundation Resources in Layer 1 SHALL NOT BE MODIFIED.

Base Resources in Layer 2 MAY BE MODIFIED, but SHOULD BE extended by reference whenever feasible.

Instead of Clinical Resources, Layer 3 includes MODIFIED OR ADDITIONAL resources that are used for Digital Rights. Examples include Remediation in place of Medication, etc.

For Digital Rights, Layer 4 represents the enforcement aspect of implementing policies and controls.
Instead of supporting the costing, financial transactions and billing which occur within and between healthcare providers, insurers and patients in FHIR, the Digital Rights implementation supports filing complaints and/or lawsuits, terminating relationships, testing for violations and effectiveness of remediation controls,
and publishing of evidence in a privacy-preserving manner for the benefit of investigators, auditors, and any other relevant party.

Specialized Resources in layer 5, and Resource Contextualization in Layer 6 is where MOST MODIFICATIONS should be made for Digital Rights implementations. For instance, although the following resources are defined in Layer 4 of FHIR, their modified versions should be defined or constrained by Layer 5 or 6.  

    Account - used when an online portal or financial billing account is established between the individual and an organization.

    ActivityDefinition - used to define tasks and events associated with digital rights.
    
    AdverseEvent - used to classify actual or potentially harmful events.
    
    AllergyIntolerance - could be used to represent an intolerance for any vulnerability, risk, control, or remediation. 
    
    AuditEvent - an entry in a log or database that records important events. Can use any custom CodeSystem to define and configure which events are recorded and how notifications are generated. Example: Google Cloud Platform Healthcare API has a field for "Agent", and uses native Pub-Sub Notifications not only for core FHIR events, but also for events in the surrounding host environment.
    
    Basic - used as a base resource for Narrative content which can include custom in-line schemas. The benefit of using this resource over using an '<any>' node, is that typical Search and List functions are supported. 
    
    Binary - used for MIME content types usually associated with attachments. There are no restrictions on content type, but this resource cannot be searched and will not be explicitly returned in GET calls on related resources.
    
    CarePlan - used to represent a dataSubject's Personal Privacy Program.
    
    CareTeam - used to manage agents, advisors, etc. who assist the dataSubject in accomplishing Goals in the CarePlan.
    
    CatalogEntry - used for documenting published evidence and test results in a data catalog.
    
    ChargeItem - used for quantifying cost of actions, events, harm, etc.
    
    ChargeItemDefinition - used to identify and code ChargeItems.
    
    Claim - used for complaints, lawsuits, etc. which are generally shared with enforcement authorities and legal professionals. Includes monetary fields for damages and legal fees.
    
    ClaimResponse - used by enforcement authorities to provide status information about a Claim, such as whether a Claim was awarded or denied. Can also reference ExplanationOfBenefit or CoverageEligibility resources for additional details.
    
    ClinicalImpression - could be used by members of a CareTeam to record observations upon which recommendations are made.
    
    CodeSystem - used as a reference table to any distinct ontology, vocabulary, standard, statute, etc.
    
    Communication - used to 1) document verbal or physical encounters/interchanges which are not electronic/digital; or 2) used to flag a digital interchange message with high importance.
    
    CommunicationRequest - similar to an Inquiry, except it has no specific recipient, and therefore its content should be considered possibly sensitive. Typically used for communication failures, as in "Houston, we have a problem communicating. Please acknowledge."  
    
    CompartmentDefinition - used to group resources for access control or to facilitate search operations. Example: define a liited dataset of public PII typically used to populate most contact and registration webforms.
    
    ConceptMap - used to map different CodeSystems to each other, such as CCPA to GDPR, etc.
    
    Condition - Examples: 1) CareTeam assess dataSubject is breached 20 times more often than a given population; 2) Excessive number of credentials pwn'd. 
    
    Consent - heavily-used resource for documenting permissions and all auditable events. Also serves as a quantifiable metric for CarePlan and RiskAssessment resources. 
    
    Contract - used when the business relationship is contractual, or when there is no explicit business contract, a privacy policy, terms of service, or NDA can be represented as a contract.
    
    Coverage - used to represent a Data Processing Agreement (DPA), or a set of explicit consents, or the relevant portions of a privacy policy.
    
    CoverageEligibilityRequest - used as an Inquiry when coverage is not specified or is ambiguous. Typically sent to Vendors when their exemption status to a law is unclear, or possibly sent to enforcement authorities to determine legal standing of a Claim. 
    
    CoverageEligibilityResponse - used as a response to this Inquiry, this resource can be validated for information required in the response.
    
    DetectedIssue - used to flag any auditable or non-auditable event warranting additional scrutiny. Not to be interpreted as an AdverseEvent, but more as a possible issue to triage.
    
    Device - used often to register or associate a physical object which hosts an Endpoint for common messaging interchanges.
    
    DeviceDefinition - used to define each class or manufacturer of a Device, and includes DeviceMetrics.
    
    DeviceRequest - could be used to document every instance in which permission to access is requested, such as: to use your camera, access your photos, etc.
    
    DeviceUseStatement - could be used to register or associate a device with a dataSubject - or used to document a Vendor's policy or terms of use for the device.
    
    DiagnosticReport - used by CareTeam to summarize and analyze Conditions, ClinicalImpressions, or RiskAssessment when creating or modifying a CarePlan.
    
    EffectEvidenceSynthesis - used to document the probability of AdverseEvents in RiskAssessments which reference research studies and datasets for comparative purposes.
    
    Encounter - used to represent a logical information exchange between any entity.
    
    Endpoint - used to associate ports/addresses/apps/operations with a Device. Also contains a ContactPoint resource in case the Endpoint changes or is not functional.
    
    EnrollmentRequest - used to document and request participation in non-digital registration, or when a non-standard resource is involved. A typical use-case scenario is for initial registration steps in which a digital operation is not yet available. 
    
    EnrollmentResponse - typically used as input to digital enrollments/registrations when requests are granted.
    
    EpisodeOfCare - used to documetn activities outside of cor FHIR. Examples: dataSubject freezes credit; works with another agent, or receives help from an acor outside of FHIR.
    
    EventDefinition - used to document details about events logged and unlogged. and is typically used for generating Notifications.
    
    Evidence - used as a container for a set of EvidenceVariables which could be submitted from any actor, such as dataSubject, Vendor, Enforcement Authority, etc.
    
    ExampleScenario - can be used to describe steps for exercising digital rights with a given Vendor; or to provide examples of violations found or mandated remediations from an enforcement authority; or, as part of a request, such as a purchasing Inquiry, to better explain the nature or motivation of a request.  
    
    ExplanationOfBenefit - can be used by an enforcement authority to explain why a Claim was approved or denied, and may record complaint details, investigation findings, evidence considered, and actions taken. Can also be used by a Vendor to document why a request was denied either partially or in full; or if the request was approved, an explanation of the fulfillment deliverable.
    
    FamilyMemberHistory - used to bundle a set of Conditions, AdverseEvents, and other datasets for the principal dataSubject and their RelatedPersons over a given time period, facilitating search and retrieval functions. Currently, there is no other resource aside from Group to document members of a family or household.
    
    Flag - used to mark a DetectedIssue associated with a resource, action, or event. Example: excessively high risk rating or poor test results can be annotated on a Vendor record or used to generate a Notification. 
    
    Goal - used to address a condition or remediation, typically as part of a CarePlan. Examples: 1) reduce exposure to unauthorized data sharing; 2) hold non-responsive Vendors accountable; 3) what a remediation or control is intended to do.
    GraphDefinition - typically used to define or link relationships between actors such as dataSubjects, Vendors, dataBrokers, etc. Also can be used for entity resolution of complex or shady enterprises. Includes rules for governing relationships and data sharing. Heavily used to discover unauthorized data sharing via tracking technologies and/or lax permissions.
    
    Group - used to represent analytical population sets for comparing individual members with others in the same group having common characteristics, or outside of the group.
    
    GuidanceResponse - can be used by a CareTeam member to advise dataSubject regarding a Vendor's RiskAssessment; or used byan Enforcement Authority to represent a remediation recommendation.
    
    Invoice - used to bill for ChargeItems.
    
    Library - used to reference knowledge-based assets in any resource.
    
    Location - used as a logical representation of a physical or virtual location, which includes physical addresses, IP addresses, relative or directional descriptions (inside, across-from, etc.), or named places, such as a landmark or nation-state.
    
    Measure - [TODO]
    
    MeasureReport - [TODO]
    
    Media - [TODO]
    
    Medication - used to represent a Remediation.
    
    MedicationRequest - used to request a remediation for a Vulnerability or Violation. An example of this Resource used for a Violation, is what an Enforcement Authority would request of an Organization that violated a law. Also can be used to request a remediation for a Vulnerability when there is no Violation of any law, but could be a high risk if left unmanaged.
    
    MedicationDispense - used to represent an instance of a Remediation, which can also include prescriptive (dosage) guidelines, i.e. how often this remediation should occur within a given timeframe.
    
    MedicationAdministration - The remediations planned or prescribed to correct violations.
    
    MedicationStatement  - used to document the personal constraints or preferences used to process requests or manage vendors.
    
    MedicationKnowledge - contains or references information about specific remediations, such as conditions, standards, and technical requirements, or a list of controls.

    MedicinalProduct - [TODO]
    
    MedicinalProductAuthorization - [TODO]
    
    MedicinalProductContraindication - [TODO]
    
    MedicinalProductIndication - [TODO]
    
    MedicinalProductIngredient - [TODO]
    
    MedicinalProductInteraction - [TODO]
    
    MedicinalProductManufactured - [TODO]
    
    MedicinalProductPackaged - [TODO]
    
    MedicinalProductPharmaceutical - [TODO]
    
    MedicinalProductUndesirableEffect - [TODO]
    
    MolecularSequence - [TODO]
    
    NamingSystem - [TODO]
    
    NutritionOrder - [TODO]
    
    Observation - used to document evidence that is anecdotal or non-quantitative in nature - think "contemporaneuous notes".
    
    ObservationDefinition - used to provide additional details or context about an Observation.
    
    Organization - [TODO]
    
    OrganizationAffiliation - [TODO]
    
    Patient - [TODO]
    
    PaymentNotice - used to notify an Organization that a complaint or suit has been filed in a jurisdiction.
    
    PaymentReconciliation - used to record the Organization's response to a complaint or lawsuit.
    
    Person - used to represent a dataSubject that is not the principal dataSubject. It could be an employee at a business, a member of a social group, etc. This resource is important when redacting identifiable information.
    
    PlanDefinition - used to provide additional details or context around a CarePlan or VulnerabilityPlan.
    
    Practitioner - used to represent a party who acts as a Processor of personal information. Could be a Vendor or a ThirdParty.
    
    PractitionerRole -used to classify the Party's role. Example: whether the Processor is a Vendor, ThirdParty, Mediator, DSAR-provider, etc.
    
    Procedure - An action that is performed, planned to be performed, or was performed by any Party.
    
    Provenance - used without modification to describe entities and processes involved in producing and delivering information. Used for assessing authenticity, enabling trust, and allowing reproducibility.
    
    Questionnaire - [TODO]
    
    QuestionnaireResponse - [TODO]
    
    RelatedPerson - [TODO]
    
    RequestGroup - [TODO]
    
    ResearchDefinition - [TODO]
    
    ResearchElementDefinition - [TODO]
    
    ResearchStudy - [TODO]
    
    ResearchSubject - [TODO]
    
    RiskAssessment - used to evaluate risk exposure from any Party, typically by analyzing test results and other evidence.
    
    RiskEvidenceSynthesis - typically used to evaluate residual risk after a control is applied or a VulnerabilityPlan is completed.
    
    Schedule - [TODO]
    
    ServiceRequest - [TODO]
    
    Slot - [TODO]
    
    Specimen - [TODO]
    
    SpecimenDefinition - [TODO]
    
    Subscription - used to identify actual services or information pushed from Vendor to DataSubject.
    
    Substance - [TODO]
    
    SubstanceReferenceInformation - [TODO]
    
    SubstanceSpecification - [TODO]
    
    SubstanceSourceMaterial - [TODO]
    
    SupplyDelivery - used as an Event Definition topic when Fullfillment of a Privacy Request is posted.
    
    SupplyRequest - used to request additional details which were not fulfilled or were not present in the original Privacy Request.
        
    Task - typically used as a "sub-Action" when all or a portion of a procedure is outsourced to a person, organization, or service outside of the target Party's domain.
    
    TerminologyCapabilities - [TODO]
    
    VerificationResult - used for specific operations which need to be verified prior to proceeding, such as identity or fulfillment.
    
    VisionPrescription - used as an authorization for the individual's agent.

============================
### Topic: REST API Overview

**FHIR implementation**

A FHIR REST server is any software that implements the FHIR APIs and uses FHIR resources to exchange data. 
The diagram below describes the FHIR interface definitions. The methods are classified as:

iServeInstance - methods that perform Get, Put or Delete operations on a resource

iServeType - methods that get type information or metadata about resources

iServeSystem - methods that expose or enable system behaviors.

This architecture supports Segregation of Duties (SoD) for roles like: Data Steward, Developer, SysAdmin.

FHIR is described as a 'RESTful' specification based on common industry level use of the term REST. 
In practice, FHIR only supports Level 2 of the REST Maturity model as part of the core specification, 
though full Level 3 conformance is possible through the use of extensions. 
Because FHIR is a standard, it relies on the standardization of resource structures and interfaces. 
This may be considered a violation of REST principles but is key to ensuring consistent interoperability across diverse systems.

Each "resource type" has the same set of interactions defined that can be used to manage the resources in a highly granular fashion. Applications claiming conformance to this framework claim to be conformant to "RESTful FHIR" (see Conformance). Note that in this RESTful framework, transactions are performed directly on the server resource using an HTTP request/response. The API does not directly address authentication, authorization, and audit collection. All interactions support synchronous and asynchronous use patterns.

The API describes the FHIR resources as a set of operations (known as "interactions") 
on resources where individual resource instances are managed in collections by their type. 
Servers can choose which of these interactions are made available and which resource types they support. 
Servers MUST provide a Capability Statement that specifies which interactions and resources are supported. 

In addition to a number of General Considerations this page defines the following interactions:

  **Instance Level Interactions**	
  
    read - Read the current state of the resource.
    
    vread	- Read the state of a specific version of the resource.
    
    update - Update an existing resource by its id (or create it if it is new).
    
    patch	- Update an existing resource by posting a set of changes to it.
    
    delete - Delete a resource.
    
    history	- Retrieve the change history for a particular resource.
    

  **Type Level Interactions**
  
    create - Create a new resource with a server assigned id.
    
    search - Search the resource type based on some filter criteria.
    
    history	- Retrieve the change history for a particular resource type.

  **Whole System Interactions**
  
    capabilities - Get a capability statement for the system.
    
    batch/transaction - Update, create or delete a set of resources in a single interaction.
    
    history	- Retrieve the change history for all resources.
    
    search - Search across all resource types based on some filter criteria.
    

In addition to these interactions, there is an operations framework, which includes endpoints for validation, messaging and Documents. Also, implementers can use GraphQL.

**Support for HEAD**

Anywhere that a GET request can be used, a HEAD request is also allowed. 
HEAD requests are treated as specified in HTTP: same response as a GET, but with no body.

Servers that do not support HEAD MUST respond in accordance with the HTTP specification, 
for example using a 405 ("method not allowed") or a 501 ("not implemented").

**Custom Headers**

This specification defines or recommends some custom headers that implementers can use to assist with deployment/debugging purposes:

X-Request-Id - A unique id to for the request/response assigned by either client or server. Request: assigned by the client. Response: assigned by the server.

X-Correlation-Id - A client assigned request id echoed back in the response.

X-Forwarded-For - Identifies the originating IP address of a client to an intermediary.

X-Forwarded-Host - Identifies the original host requested by the client in the Host HTTP request header.

X-Intermediary - Stamped by an active intermediary that changes the request or the response to alter it's content.

The request id in X-Request-Id is purely to help connect between requests and logs/audit trails. 
The client can assign an id to the request, and send that in the X-Request-Id header. 
The server can either use that id or assign it's own, which it returns as the X-Request-Id header in the response. 
When the server assigned id is different to the client assigned id, the server SHOULD also return the X-Correlation-Id header with the client's original id in it.

The HTTP protocol may be routed through an HTTP proxy (e.g. as squid). Such proxies are transparent to the applications, though implementers should be alert to the effects of caching, particularly including the risk of receiving stale content. 

Interface engines may also be placed between the consumer and the provider. These differ from proxies because they actively alter the content and/or destination of the HTTP exchange and are not bound by the rules that apply to HTTP proxies. Such agents are allowed, but SHALL mark the request with an X-Intermediary header to assist with debugging/troubleshooting.

Any agent that modifies an HTTP request or response content other than under the rules for HTTP proxies 
SHALL add a stamp to the HTTP headers like this:

  X-Intermediary : [identity - usually a FQDN]
End point systems SHALL NOT use this header for any purpose. 
Its aim is to assist with system troubleshooting. 

**Digital Rights implementation**

An Authorized Agent could use this header to correlate requests and responses for multiple clients 
(group of DataSubjects authorizing Agent to act on their behalf), act as a proxy by forwarding messages to the intended endpoint, and/or act as an interface engine to alter headers and content.
This is in direct conflict with FHIR's mandate that "End point systems SHALL NOT use this header for any purpose."

============================
### Topic: REST API Transactions

**FHIR implementation**

As mentioned, FHIR resources are optimized for stateless transactions with RESTful APIs. 
Although this is not the only way FHIR resources can be used, these types of transactions are the only ones with defined interfaces and behaviors in the FHIR specification.

FHIR transactions follow a simple request and response transaction pattern. 
The request and response can be for a single payload or can operate as batch. 
The payload or a request and response consist of a header and the content of interest.

For example, a Bundle represents a simple request and response as a Layer 2 foundational Resource,
The FHIR specification only requires the 'type' attribute of a Bundle, which proves how lax the schema definition is. Not even fields like '<identifier>', '<timestamp>', '<total>' and '<entry>' are required.
This loosely-coupled artifact allows external systems to process and track the state of these Bundle transactions according to rules specified within the Bundle payload content or from external systems.  

**Digital Rights implementation**
	
These transactions are used for MVP(minimally viable product) implementations.
This Implementation Guide, along with Conformance Resources and Profiles, are used to specify and validate fields required in Digital Rights implementations, such as '<identifier>', '<timestamp>', '<total>' and '<entry>'.
Underlying FHIR base schemas, such as Bundle.xsd are NEVER MODIFIED.

============================
### Topic: FHIR Extensibility and Profiling

**FHIR implementation**
	
Recognizing that a one size fits all approach is not appropriate in the healthcare space, FHIR provides the ability to adjust the forms (Resources) to be able to handle the needs of different implementation spaces by defining "extensions" as well as enforcing constraints. 
For example, a "prescription" form might have extension elements added to support tracking of restricted medications while also constraining the codes that can be used to communicate types of drugs to a particular national standard. Forms are designed in such a way that these changes can be made without changing how systems pass forms around, enabling any system to consume completed forms even if they have additional elements added, 
whether or not those additional elements are used by the receiving system.

To keep the base forms that everyone uses from being overly complex, FHIR has a rule that, in most cases, 
a resource will only include data elements if there's an expectation that most implementations will use that particular data element. 

To keep the number of resources reasonable, some of them are fairly broad. For example, the Observation resource is used for vital signs, lab results, psychological assessments and a variety of other things. 
To support setting rules for more narrow areas (e.g. "What should I send if I want to share a blood pressure?"), FHIR allows the creation of Profiles. 

**Digital Rights implementation**
	
Privacy rights requests must be usable in different countries, jurisdictions, and by different types of privacy mandates in different contexts (GDPR, COPPA, CCPA, etc.). Recognizing that a one size fits all approach is not appropriate in the privacy eco-sphere, FHIR provides the ability to adjust the forms (Resources) to be able to handle the needs of different implementations used by organizations to comply with a similar requirement or a completely different requirement, by defining "extensions" as well as enforcing constraints. 

============================
### Topic: FHIR Narrative

**FHIR implementation**
	
Although FHIR is designed to share discrete, structured data for computation, a human-readable view is also a valuable resource within an healthcare information exchange to get a full picture of what's going on.

The Narrative element is expected to exist for most resource instances, although it can be omitted in a few limited circumstances. In some cases, the narrative will be generated from discrete information. For example, the narrative for a patient might look like this:

  Peter James Chalmers (OFFICIAL), Jim
  identifier: MRN = 12345 (USUAL)
  telecom: ph: (03) 5555 6473(WORK)
  gender: MALE
  birthDate: Dec 25, 1974
  deceased: false
  address: 534 Erewhon St PleasantVille Vic 3999 (HOME)

In other cases, the narrative might be free-form text commentary entered directly by a practitioner, such as referral letters, pathology reports, etc. Certain parts of the narrative content could also later be exposed as discrete data.

These principles and requirements have led to the current approach, where the material to be rendered is placed into the Section.content. [EDITORS: The current design doesn't make it clear where to consistently find narrative.] 

**Digital Rights implementation**
	
The narrative (often a <text> element), contains the human-readable content for digital rights requests.
It can be used as a template, with data elements from a set of resources referenced as variables.
Think of the narrrative as a "representation layer", or stylesheet applied to underlying data elements from a set of resources. As a simple guidance principle, the narrative should represent ALL the raw data if not used as a template for discrete data. 

The narrative SHOULD NOT mix raw data not contained elsewhere AND raw data referenced from resource variables. If the raw data requires different levels of confidentiality or consent, use multiple sections to segment the narrative in accordance with the appropriate level of confidentiality or consent required.
	
============================
### Topic: FHIR Interfaces

**FHIR implementation**
	
In addition to defining the "forms" for data exchange (Resources), FHIR also defines a set of interfaces 
by which systems actually share that information. There are four primary mechanisms or "paradigms" of exchange supported by FHIR: 

1) via a REST interface; 2) by exchanging Documents; 3) by sending and receiving Messages, and 4) by exposing and invoking Services.

REST is the simplest exchange mechanism. With this API, you can do the following:
search, read, create, update, delete, get history, make a transaction, perform an operation.

Documents are a familiar mechanism for sharing information in the healthcare space. 
They are useful whenever there's a desire to guide how a consumer of information will navigate it 
and there's a need to have a "frozen" view of information that can be reliably retrieved even years in the future. Examples of document-like things in healthcare include discharge summaries and lab reports.

In FHIR, there's a special resource called Composition that acts as the "cover page" for a document. 
It identifies the title, author date, relevant patient and the table of contents. 
A FHIR document can be thought of as a set of sheets (Resource instances) stacked together with a title page on top that's stapled together. That stapled collection can then be stored or passed around, conveying a complete set of information at once.

Much healthcare information exchange happens using a messaging paradigm. 
In messaging, a set of information is sent from one system to another, typically triggered by an event in the sender system. For example, a patient being admitted, a lab test being ordered, a drug being administered, 
the clock striking 12:00 or someone pressing a button. The message serves to notify the receiver that the event occurred as well as provide details about any existing data that was modified or new data that was created. 
Typically receiving a message means there's an expectation that the receiving system will "do something" in response.

A message might request that a lab order be fulfilled or notify a system that two patient records have been merged or that a patient has been transferred from one bed to another. 
A message is similar to a document in that it collects resources together, however for a message, 
the "cover page" is a MessageHeader that acts as a requisition. And rather than using a staple, the resources are joined together with a paper-clip and there's no expectation that the receiving system will store the contents of the message exactly as received, if at all.

Services can be thought of as a light-weight way of doing messaging. Rather than a full cover page, 
a small sticky note is attached to the front of a resource. And sometimes rather than sending a full piece of paper, the relevant pieces are cut out and sent as fragments. The response to a requisition is a similarly paper-clipped bundle of resource instances. Services are likely to be used for things like decision support. 
E.g. "Is there a problem with prescribing medication X for patient Y?" or "What's the recommended care plan for a patient with conditions A, B and C?"

**Digital Rights implementation**
	
Because there is no current standard for a Digital Rights or AgentAPI, it's necessary to use exchange methods which do not depend on HTTP REST APIs. For example, an Access Request can be sent to an Organization via an email template that pulls in the Narrative and other personal details from FHIR resources.
Responses to the Access Request from the Organization can be validated against the appropriate Response message, document, or service.
FHIR does not define interfaces for other transport protocols such as SMTP, telephony, or webforms,
but these methods are needed in order to exercise privacy rights over the "proper channels" dictated by the Organization.
	
============================
### Topic: FHIR Clinical Resources

**FHIR implementation**
	
A FHIR-based system's capabilities are defined by what the Resources can say and from a clinical perspective, 
these things define the clinical record:
- the kinds of Resources that are defined.
- their data contents, and rules about the data such as what terminology codes are supported and/or required.
- how resources reference to each other.
- how you can search for information.
This information can all be found in the resource definition pages. 

**Digital Rights implementation**
	
In Digital Rights, a FHIR-based system's capabilities are also defined by what the Resources say, except that instead of a clinical record, what is being defined is the collection of a DataSubject's personal information that is collected, stored, processed and shared by each "Party", such as an Organization or Enforcement Authority.

As a modified set of FHIR Resource definitions, this includes:

Clinical & Care provision - The privacy rights which are "covered" by legislation and contract law.

Diagnostics - The privacy "issues" being addressed through privacy rights requests.

Medications - The remediations planned or prescribed to correct violations.

Administrative - The personal constraints or preferences used to process requests or manage vendors.

============================
### Topic: FHIR Developers Overview

**FHIR implementation**
	
FHIR is based on "Resources" which are the common building blocks for all exchanges. Resources are an instance-level representation of some kind of healthcare entity. All resources have the following features in common:
-  A URL that identifies the resource.
-  Common metadata.
-  A human-readable XHTML summary.
-  A set of defined data elements - a different set for each type of resource.
-  An extensibility framework to support variation in healthcare.
Resource instances are represented as either XML, JSON or RDF.

Example Resource Instance 
This is an example of how a patient is represented as a FHIR object in JSON. An XML encoding is also defined in the specification.

{
  "resourceType": "Patient",
  "id" : "23434",
  "meta" : {
    "versionId" : "12",
    "lastUpdated" : "2014-08-18T15:43:30Z"
  }
  "text": {
    "status": "generated",
    "div": "<!-- Snipped for Brevity -->"
  },
  "extension": [
    {
      "url": "http://example.org/consent#trials",
      "valueCode": "renal"
    }
  ],
  "identifier": [
    {
      "use": "usual",
      "label": "MRN",
      "system": "http://www.goodhealth.org/identifiers/mrn",
      "value": "123456"
    }
  ],
  "name": [
    {
      "family": [
        "Levin"
      ],
      "given": [
        "Henry"
      ],
      "suffix": [
        "The 7th"
      ]
    }
  ],
  "gender": {
    "text": "Male"
  },
  "birthDate": "1932-09-24",
  "active": true
}


Each instance of a resource consists of:

resourceType (line 2) - Required: FHIR defines many different types of resources.

id (line 3) - The id of this resource. Always present when a resource is exchanged, except during the create operation (below).

meta (lines 4 - 7) - Usually Present: Common use/context data to all resources and managed by the infrastructure. Missing if there is no metadata

text (lines 8 - 11) - Recommended: XHTML that provides a human readable representation for the resource
extension (lines 12 - 17) - Optional: Extensions defined by the extensibility framework

data (lines 18 - 43) - Optional: data elements - a different set, defined for each type of resource

**URLs and Identities**

All resources may have a URL that identifies the resource and specifies where it was/can be accessed from. 
This URL is not represented inside the resource; the value arises in a context use, and changes as copies of the resource are made, or following other deployment/security related changes. 
If the resource is accessed via the FHIR RESTful API then the URL for the resource is [base]/[resourceType]/[id] where the resourceType and id come from the resource.

Some resources have an explicit URL in them, which is normally the URL of master copy of the resource. 
This URL remains constant as the resource is copied across systems.

**Interactions**

For manipulation of resources, FHIR provides a REST API with a rich but simple set of interactions:

Create = POST https://example.com/path/{resourceType}

Read = GET https://example.com/path/{resourceType}/{id}

Update = PUT https://example.com/path/{resourceType}/{id}

Delete = DELETE https://example.com/path/{resourceType}/{id}

Search = GET https://example.com/path/{resourceType}?search parameters...

History = GET https://example.com/path/{resourceType}/{id}/_history

Transaction = POST https://example.com/path/ (POST a transaction bundle to the system)

Operation = GET https://example.com/path/{resourceType}/{id}/${opname}


The FHIR specification describes other kinds of exchanges beyond this simple RESTful API, 
including exchange of groups of resources as Documents, as Messages, and by using various types of Services.

**Managing Variability**

There is a wide variation between different geo-political jurisdictions and segments of the healthcare industry, and no central authority to impose common business practices. Because of this, the FHIR specification defines an extension framework and defines a framework for managing variability.

Another key aspect of the variability encountered in healthcare is that the same information may be represented differently and with different levels of detail, granularity and nesting by various parties across the system. 
For example, in some cases a blood pressure measurement may be just a simple observation, 
a vital sign measure, while in other cases can be a rich set of highly defined data that includes things like controlled vocabularies for posture, exercise, etc. The resource types defined in this specification focus on the general, common use cases. Richer and more specific content can be supported and standardized by defining "profiles" on the base resource types.

**Managing Versions**

Versions, in the context of FHIR, means one of three different things:

- FHIR Version: Which FHIR version is in use?

- Record Version: for tracking changes to resources, and preventing changes overwriting each other.

- Business Version: So humans know which version of the content they are dealing with (for some kinds of resources).

- FHIR Version.

Usually, the FHIR version is fixed by the context - the CapabilityStatement that a client can use to find out about the server, but there are other ways of managing multiple FHIR versions.

**Record Version**

FHIR Servers do not have to support versioning, though they are strongly encouraged to do so. 
There are three different levels of versioning support for FHIR servers:

1) Versioning and .meta.version are not supported (and usually, .meta.lastUpdated is not supported either).

2) Versioning and the VersionId meta-property are supported, but a history of old versions is not kept.

3) Versioning and the VersionId meta-property are supported, and a history of old versions is available.

**Business Version**

Some resources - typically those that represent content that goes through a formal publishing cycle 
- carry a version element that explicitly states what version of the content the resource represents. 
This is changed explicitly by a human, or by some automated process in accordance with applicable business rules.

**Creating a resource**

To create a resource, send an HTTP POST request to the resource type's respective end point.

  POST https://example.com/path/{resourceType}

  
In the example below we see the creation of a Patient:

  POST {some base path}/Patient HTTP/1.1
  Authorization: Bearer 37CC0B0E-C15B-4578-9AC1-D83DCED2B2F9
  Accept: application/fhir+json
  Content-Type: application/fhir+json
  Content-Length: 1198
 
{
  "resourceType": "Patient",
  ...(properties)
}

Submit a new patient to the server, and ask it to store the patient with an id of its own choice:

Patient (line 1) - the manager for all patients - use the name of the type of resource.

Authorization (line 2) - see Security for FHIR.

Accept, Content-Type (lines 3-4) - the content type for all FHIR resources as represented in JSON (or application/fhir+xml for the XML version). FHIR resources are always represented in UTF-8.

id - The client does not need to provide an id for a resource that is being created - the server will assign one. If the client assigns one, the server will overwrite it.

Resource Content, lines 8+ - There's no meta property at this point. The rest of the resource is the same content as shown above.


**Create Response**

A response will contain an HTTP code 201 to indicate that the Resource has been created successfully. 
A location header indicates where the resource can be fetched in subsequent requests. 
The server may choose to return an OperationOutcome resource, but is not required to do so.

  HTTP/1.1 201 Created
  Content-Length: 161
  Content-Type: application/fhir+json
  Date: Mon, 18 Aug 2014 01:43:30 GMT
  ETag: W/"1"
  Location: http://example.com/Patient/347
 
{
  "resourceType": "OperationOutcome",
  "text": {
    "status": "generated",
    "div": "<div xmlns=\"http://www.w3.org/1999/xhtml\">The operation was successful</div>"
  }
}

HTTP/1.1 201 (line 1) - the operation was successful. Note that use of HTTP v 1.1  is strongly recommended but not required.

ETag (line 5) - used in the version aware update pattern (if the server supports versioning).

Location (line 6) - the id the server assigned to the resource. The id in the URL must match the id in the resource when the resource is subsequently returned.

OperationOutcome (line 9) - OperationOutcome resources in this context have no id or meta element (they have no managed identity).

**Error response** 
For a variety of reasons, servers may need to return an error. Clients should be alert to authentication related responses, but FHIR content related errors should be returned using an appropriate HTTP status code, with an OperationOutcome resource to provide additional information. 

Here is an example of a server rejecting a resource because of server defined business rules:

  HTTP/1.1 422 Unprocessable Entity
  Content-Length: 161
  Content-Type: application/fhir+json
  Date: Mon, 18 Aug 2014 01:43:30 GMT
 
{
  "resourceType": "OperationOutcome",
  "text": {
    "status": "generated",
    "div": "<div xmlns=\"http://www.w3.org/1999/xhtml\">MRN conflict
   - the MRN 123456 is already assigned to a different patient</div>"
  },
}

The server can return additional structured information using the details of the OperationOutcome

**Read Request**

Reading a resource is done by sending HTTP GET requests to the desired Resource Type end-point.

  GET https://example.com/path/{resourceType}/{id}

Here's an example:

  GET /Patient/347?_format=xml HTTP/1.1
  Host: example.com
  Accept: application/fhir+xml
  Cache-Control: no-cache

347 (line 1) - The id of the resource that is being fetched.

_format=xml (line 1) - this is another method for clients to indicate the desired response format, in addition to using the accept header, and is useful for clients that don't have access to the HTTP Headers (e.g. XSLT transforms).

cache control (line 4) - Cache control is important, though FHIR itself says nothing about it.


**Read Response**

The response to a GET contains the Resource.

  HTTP/1.1 200 OK
  Content-Length: 729
  Content-Type: application/fhir+xml
  Last-Modified: Sun, 17 Aug 2014 15:43:30 GMT
  ETag: W/"1"
 
<?xml version="1.0" encoding="UTF-8"?>
<Patient xmlns="http://hl7.org/fhir">
  <id value="347"/>
  <meta>
    <versionId value="1"/>
    <lastUpdated value="2014-08-17T15:43:30Z"/>
  </meta>
  <!-- content as shown above for patient -->  
</Patient>

id (line 9) - The id of the resource. This must match the id in the read request.

versionId (line 11) - The current version id of the resource (if the server supports versioning). Best practice is that this value matches the ETag (see version aware update), but clients must never assume this.

lastUpdated (line 12) - if present, this must match the value in the HTTP header.


**Search Request**

In addition to getting single known resources it's possible to find a collection of resources by searching the resource type end-point with a set of criteria describing the set of resources that should be retrieved, and their order. The general pattern is:

  GET https://example.com/path/{resourceType}?criteria

The criteria is a set of HTTP parameters that specify which resources to return. 
The search operation https://example.com/base/MedicationRequest?patient=347 returns all the prescriptions for the patient created above.

**Search Response**

The response to a search request is a Bundle: a list of matching resources with some metadata:

  HTTP/1.1 200 OK
  Content-Length: 14523
  Content-Type: application/fhir+xml
  Last-Modified: Sun, 17 Aug 2014 15:49:30 GMT
 
{
  "resourceType": "Bundle",
  "type": "searchset",
  "id" : "eceb4882-5c7e-4ca4-af62-995dfb8cef01"
  "timestamp": "2014-08-19T15:49:30Z",
  "total": "3",
  "link": [
    {
      "relation" : "next",
      "url" : "https://example.com/base/MedicationRequest?patient=347&searchId=ff15fd40-ff71-4b48-b366-09c706bed9d0&page=2"
    }, {
      "relation" : "self",
      "url" : "https://example.com/base/MedicationRequest?patient=347"
    }
  ],
  "entry": [
    {
      "resource" : {
        "resourceType": "MedicationRequest",
        "id" : "3123",
        "meta" : {
          "versionId" : "1",
          "lastUpdated" : "2014-08-16T05:31:17Z"
        }, 
        ... content of resource ...
      }, 
    }, 
    ... 2 additional resources ....
  ]
}

resourceType/type (line 7/8) - the result of a search is always a bundle of type "searchset".

id (line 9) - An identifier assigned to this particular bundle. The server should assign a unique id to this bundle that it will not be re-used.

timestamp (line 11) - (if the server supports versioning) This should match the HTTP header, and should be the date the search was executed, or more recent, depending on how the server handles ongoing updates. The timestamp must be the same or more recent than the most recent resource in the results 

total (line 13) - The total number of matches in the search results. Not the number of matches in this particular bundle, which may be a paged view into the results.

link (line 14) - A set of named links that give related contexts to this bundle. Names defined in this specification: first, prev, next, last, self.

entry (line 23) - Actual resources in this set of results.

entry.resource.id (line 27) - Note that in some bundles, the entry.resource.id must be unique in the bundle.


**Update Request**

The client sends the server a new version of the resource to replace the existing version - it PUTs it to the location of the existing resource:

  PUT https://example.com/path/{resourceType}/{id}

Note that there does not need to be a resource already existing at {id} - the server may elect to automatically create the resource at the specified address. Here is an example of updating a patient:

  PUT /Patient/347 HTTP/1.1
  Host: example.com
  Content-Type: application/fhir+json
  Content-Length: 1435
  Accept: application/fhir+json
  If-Match: 1
 
{
  "resourceType": "Patient",
  "id" : "347",
  "meta" : {
    "versionId" : "1",
    "lastUpdated" : "2014-08-18T15:43:30Z"    
  },
  ...
}

resourceType (line 1) - "Patient" in the URL must match the resource type in the resource (line 9).

resource id (line 1, "347") - This must match the id in the resource (line 10).

If-Match (line 6) - if this is provided, it must match the value in meta.versionId (line 12), and the server must check the version integrity, or return 412 if it doesn't support versions.

meta.lastUpdated (line 13) - This value is ignored, and will be updated by the server (mostly, but not always, if the server does not support versioning).

resource content (line 14) - Not shown here, the same as Patient above.


**Update Response**

The response to an update request has metadata / status, and optionally an OperationOutcome:

  HTTP/1.1 200 OK
  Content-Length: 161
  Content-Type: application/fhir+json
  Date: Mon, 18 Aug 2014 01:43:30 GMT
  ETag: W/"2"
  Location: https://example.com/Patient/347/_history/2
 
{
  "resourceType": "OperationOutcome",
  "text": {
    "status": "generated",
    "div": "<div xmlns=\"http://www.w3.org/1999/xhtml\">The operation was successful</div>"
  }
}

ETag (line 5) - This is the versionId of the new version, and is also found in the location header (line 6) (if the server supports versioning),

Base Resource Content 
Here is an example that shows all the information found in all resources, fully populated:

{
  "resourceType" : "X",
  "id" : "12",
  "meta" : {
    "versionId" : "12",
    "lastUpdated" : "2014-08-18T15:43:30Z",
    "profile" : ["http://example-consortium.org/fhir/profile/patient"],
    "security" : [{
      "system" : "http://terminology.hl7.org/CodeSystem/v3-ActCode",
      "code" : "EMP"
    }],
    "tag" : [{
      "system" : "http://example.com/codes/workflow",
      "code" : "needs-review"
    }]
  },
  "implicitRules" : "http://example-consortium.org/fhir/ehr-plugins",
  "language" : "X"
}

resourceType (line 2) - always found in every resource. In XML, this is the name of the root element for the resource.

id (line 3) - defined when the resource is created, and never changed. Only missing when the resource is first created.

meta.versionId (line 5) - changes each time any resource contents change (except for the last 3 elements in meta - profile, security and tag).

meta.lastUpdated (line 6) - Changes when the versionId changes. Systems that don't support versions usually don't track lastUpdated either.

meta.profile (line 7) - An assertion that the content conforms to a profile. Can be changed as profiles and value sets change or the system rechecks conformance.

meta.security (lines 8 - 11) - Security labels applied to this resource. These tags connect resources in specific ways to the overall security policy and infrastructure. Security tags can be updated when the resource changes, or whenever the security sub-system chooses to.

meta.tag (lines 12 - 16) - Tags applied to this resource. Tags are used to relate resources to process and workflow. Applications are not required to consider the tags when interpreting the meaning of a resource

implicitRules (line 17) - indicates that there is a custom agreement about how the resources are used that must be understood in order to safely process the resource. Use of this is discouraged because it restricts sharing, but sometimes necessary.

language (line 18) - The base language of the resource. The resource is allowed to have content from other languages; this is just the base, but should be the main language of the resource. The base properties of all resources are defined on the resource types Resource and DomainResource.

**Digital Rights implementation**

No specific guidance provided at this time.
	
============================
### Topic: FHIR Security and Privacy Module

**FHIR implementation**
	
The Security and Privacy Module describes how to protect a FHIR server (through access control and authorization), how to document what permissions a user has granted (consent), and how to keep records about what events have been performed (audit logging and provenance). 

FHIR does not mandate a single technical approach to security and privacy; rather, the specification provides a set of building blocks that can be applied to create secure, private systems.

The Security and Privacy module includes the following:

Resources 	Datatypes 	Implementation Guidance and Principles

---------   ---------   --------------------------------------

Consent     Signature   Security Principles

Provenance              Security Labels

Audit Event             Signatures


Common use-cases for the Security and Privacy Module:

    Security and Privacy
    
    Authorization and Access Control
    
    Authorization considerations with Query Parameters
    
    User Identity and Access Context
    
    Security and Privacy Audit Logging
    
    Accounting of Disclosures and Access Reports
    
    Privacy Consent
    
    Provenance
    
    Digital and Electronic Signatures
    
    De-Identification, Anonymization, and Pseudonymization
    
    Security Considerations on Test Data


**Security**

FHIR is focused on the data access methods and encoding leveraging existing Security solutions. Security in FHIR needs to focus on the set of considerations required to ensure that data can be discovered, accessed, or altered only in accordance with expectations and policies. Implementation should leverage existing security standards and implementations to ensure that:

- All communications can be encrypted to prevent unauthorized access.

- No information leaks when errors occur.

- No active script content can be injected into narrative resources.

- Full audit trails can be constructed and used to detect anomalous access patterns. 

**Privacy**

Privacy in FHIR includes the set of considerations required to ensure that individual data are treated according to an individual's Privacy Principles and Privacy-By-Design. FHIR includes implementation guidance to ensure that:

- Individual preferences can be communicated through standards-based protocols (e.g., OAuth, User-Managed Access) or using an explicit FHIR representation (Consent).

- Resources can be tagged to indicate the sensitivity or confidentiality of the data they represent (Security Labels).

- Data access records and audit logs can be shared with individuals, e.g. for accounting of disclosures (Audit Event).


**Security Example Use Cases Using FHIR**

For illustrative purposes, the following diagram depicts a simple use case of a patient accessing their personal health record (portal) enabled by an underlying electronic medical record (EMR) system. The EMR plays the role of the FHIR server in this example. The pre-conditions for this use case are:

- the EMR implements the necessary FHIR APIs.

- the EMR implements the necessary authentication and authorization mechanisms.

- the patient is successfully authenticated and authorized to access FHIR resources.

The basic flow of the use case is that:

- the patient registers (if required), 

- logs in, 

- enters search criteria to identify a patient or patients of interest (the patient is most like themselves in this use case), 

- retrieves clinical documents for the patient and retrieves clinical resources for the patient. 

**Digital Rights implementation**
	
The set of considerations required to ensure that individual data are treated according to an individual's Privacy Principles and Privacy-By-Design can be implemented through a custom Conformance resource
representing the individual's privacy policy and preferences which are linked to the individual's profile.
Explicit FHIR consents can be used in lieu of a custom Conformance resource.

FHIR Consents represent rules for governing access and use of personal data elements. No other modifications are needed to support digital rights implementations.

**Security Example Use Cases Using Digital Rights**

In a simple use case of a DataSubject accessing their personal information from an external PII repository, the PII repository plays the role of the FHIR client in this example. Pre-conditions for this use case are:

- the PII repository's API maps to the necessary FHIR APIs within the Agent API;

- the PII repository's API implements the necessary authentication and authorization mechanisms;

- the DataSubject and AuthorizedAgent are successfully authenticated and authorized to access FHIR resources within the AgentAPI.

The basic flow of the use case is that:

- the DataSubject and AuthorizedAgent registers (if required), 

- logs in, 

- retrieves personal data elements for the DataSubject from the PII repository's API and supplies these as parameters to the FHIR API to populate account or identity verification forms for the DataSubject.
	
============================
### Topic: FHIR Mapping Language

**FHIR implementation**
	
The FHIR Specification includes a mapping language. The mapping language has a concrete syntax, defined and described in this page, and an abstract syntax, which is found in the StructureMap resource. 

The mapping language addresses two very different kinds of transformations:
    Structural changes between the source and target structures.
    Differences in content and formats in string (and related) primitives contained within the structures.

A map has 6 parts:

- Metadata

- Embedded ConceptMaps to translate between different code systems

- References to the structures involved in the mapping

- Imports: additional Maps used by this map

- A series of groups, each with a list of input variables

- A series of transformation rules in each group

**Digital Rights implementation**
	
The Mapping Language has a status of "Trial Use", which has potential but is not implemented at this time.

============================
### Topic: FHIR Clinical Safety

**FHIR implementation**
	
This specification defines data elements, resources, formats, methods and APIs for exchanging healthcare data 
between different participants in the healthcare process. As such, Clinical Safety is a key concern 
with regard to the specification and its many and various implementations.

**Implementer's Safety Check List**

This is a check list to help implementers be sure that they've considered all the parts of FHIR that impact on their system design with regard to safety and privacy.

**Conformance Related Safety Checks**

These basic safety checks relate to using the FHIR specification correctly:

- My system handles the full Life cycle for each resource (status codes, currency issues, and erroneous entry status).

- My system checks for modifierExtension elements of each resource.

- My system supports elements labeled as "MustSupport" in the profiles that apply to my system.

- My system has documented how distributed resource identification works in its relevant contexts of use, and where (and why) contained resources are used.

- My system manages lists of current resources correctly.

- When other systems return http errors from the RESTful API and Operations (perhaps using Operation Outcome),  my system checks for them and handles them appropriately.

- My system ensures checks for patient links (and/or merges) and handles data that is linked to patients accordingly.

- My system publishes a Capability Statement with StructureDefinitions, ValueSets, and OperationDefinitions, etc., so other implementers know how the system functions.

- All resources in use are valid against the base specification and the profiles that apply to my system.

- I've reviewed the Observation resource, and understand how focus is a mechanism for observations to be about someone or something other than the patient or subject of record.

**Date / Timezone Related Safety Checks**

- My system checks for time zones and adjusts times appropriately.

- My system renders dates safely for changes in culture and language. 

**Search Related Safety Checks**

- My system takes care to ensure that clients can (for servers) or will (for clients) find the information they need when content that might reasonably be exposed using more than one FHIR resource. 

For clients:

- My system will display warnings returned by the server to the user.

- My system checks whether the server processed all the requested search parameter, and is safe if servers ignore parameters (typically, either filters locally or warns the user).

For Servers:

- My system caters for parameters that have missing values when doing search operations, and responds correctly to the client with regard to erroneous search parameters.

- My system includes appropriate default filters when searching based on patient context - e.g. filtering out entered-in-error records, filtering to only include active, living patients if appropriate, and clearly documents these (preferably including them in the self link for a search).


**Deletion Safety Checks**

Deleting records and/or marking records as no longer valid is a well known source of safety issues in all applications. This specification allows for resources to be deleted, but does not require systems to behave in any particular fashion.

- For each resource, I have checked whether resources can be deleted, and/or how records are marked as incorrect/no longer relevant.

- Deletion of records (or equivalent updates in status) flow through the system so any replicated copies are deleted/updated.

For Servers:

- My documentation about deleted resources is clear, and my test sandbox (if exists) has deleted/error record cases in the test data.

**Privacy Related Safety Checks**

- My system checks that the right Patient consent has been granted (where applicable).

- My system sends an Accounting of Disclosure to the consenter as requested when permitted actions on resources are performed using an AuditEvent Resource.

**Security Related Safety Checks**

Basic Context:

- My system ensures that system clocks are synchronized using a protocol like NTP or SNTP, or my server is robust against clients that have the wrong clock set.

- My system uses security methods for an API to authenticate where Domain Name System (DNS) responses are coming from and ensure that they are valid.

Communications:

- Production exchange of patient or other sensitive data will always use some form of encryption on the wire.

- Where resources are exchanged using HTTP, TLS should be utilized to protect the communications channel.

- Where resources are exchanged using email, S/MIME should be used to protect the end-to-end communication.

- Production exchange should utilize recommendations for Best-Current-Practice on TLS in BCP 195

Authentication/Authorization:

- My system utilizes a risk and use case appropriate OAuth profile (preferably Smart App Launch ), with a clear policy on authentication strength.

- My system uses OpenID Connect (or other suitable authentication protocol) to verify identity of end user, 
where it is necessary that end-users be identified to the client application, and has a clear policy on identity proofing.

Access Control:
- My system applies appropriate access control to every request, using a combination of requester?s clearance (ABAC) and/or roles (RBAC).

- My system considers security labels on the affected resources when making access control decisions.

Integrity:

- My system can render narratives properly and securely(where they are used)

- My system validates all input received (whether in resource format or other) from other actors so that its data is well-formed and does not contain content that would cause unwanted system behavior.

- My system makes the right Provenance statements and AuditEvent logs, and uses the right security labels where appropriate.

**Other Safety Checks**

For Servers: 

- CORS (cross-origin resource sharing ) is appropriately enabled (many clients are Javascript apps running in a browser).

- JSON is supported (many clients are Javascript apps running in a browser; XML is inconvenient at best).

- JSON is returned correctly when errors happen (clients often don't handle HTML errors well).

- The _format header is supported correctly.

- Errors are trapped and an OperationOutcome returned.

**Digital Rights implementation**
	
In this AgentAPI, we assign the Safety Checklist to our virtual Auditor, which is represented by the VGS Control API. The checklist described above represent some of the parameters or rulesets from FHIR resources that could be sent for processing by the VGS Control API. 

============================
### Topic: FHIR Search

**FHIR implementation**
	
Search operations traverse through an existing set of resources filtering by parameters supplied to the search operation. The text below describes the FHIR search framework, starting with simple cases moving to the more complex. 

Summary Table

Search Parameter Types	Parameters for all resources	Search result parameters

Number                  _id                             _sort

Date/DateTime           _lastUpdated                    _count

String                  _tag                            _include

Token                   _profile                        _revinclude

Reference               _security                       _summary

Composite               _text                           _total

Quantity                _content                        _elements

URI                     _list                           _contained

Special                 _has                            _containedType

                        _type

In addition, there is a special search parameters _query and _filter that allow for an alternative method of searching, and the parameters _format and _pretty defined for all interactions.

Note that search parameter names are case sensitive, though this specification never defines different parameters with names that differ only in case. Clients SHOULD use correct case, and servers SHALL not define additional parameters with different meanings with names that only differ in case. 

**Digital Rights implementation**
	
This section will be updated after initial baseline tests on Search is complete.
	
============================
### Topic: Testing FHIR

**FHIR implementation**
	
As a consequence of the FHIR specification being relatively "loose", a testing framework has been established within the FHIR specification. The TestScript resource provides an implementation-agnostic description of tests that allows test execution engines to evaluate if a FHIR implementation conforms with the FHIR specification. 

The TestScript resource stands as a form of executable documentation allowing developers to examine the operations defined by the tests in order to understand how various RESTful API interactions and resources should be used in coordination. 

The TestScript resource contains:

- Name and description detailing the purpose of the test suite.

- Links describing how the test suite relates to the FHIR specification.

- A list of server interactions required to execute the test suite.

- A list of server interactions that the test suite validates the correctness of.

- The fixtures (required data or resources) the tests use during execution.

- A set of operations to set up the test suite environment.

- A list of tests each containing:

  - Name and description of the test.
  
  - Links describing how the test relates to the FHIR specification.
  
  - A list of server interactions required to execute the test.
  
  - A list of server interactions that the test validates the correctness of.

  - A list of operations that provide the execution logic of the test.
  
  - A list of assertions that provide the verification logic of the test.
  
  - A set of operations to tear down the test environment.

**Digital Rights implementation**
	
This section will be updated after initial baseline tests using TestScript is complete.

======================================

## END OF IMPLEMENTATION GUIDE
