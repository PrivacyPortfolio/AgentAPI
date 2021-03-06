# AgentAPI
draft specification

Help develop national and global standard APIs for Third-Party Agent Services! 

Agents do things on your behalf.
Agents represent actors, athletes, and artists by promoting them and negotiating contracts.
Agents represent businesses, government agencies, and non-profit organizations as lobbyists, evangelists, and industry representatives.
Agents also serve digital services, such as your web browser, or daemons that monitor events and collect information.
Some apps are Agents, consolidating your bills and bank accounts, monitoring your personal health information, or tracking the location of your family and friends.

One common characteristic of agents is that they all need:
1) authorization from the parties they represent;
2) personal data of the parties they respresent;
3) a way to share personal data with other parties.

Therefore, authorized agents are uniquely positioned to exercise privacy rights and obligations on behalf of the people or entities they represent.

This AgentAPI, is designed for use by individual consumers and their agents, or by organizational entities and their agents.
It's time we had some standards for third-party agents on the consumer side and the business side!
Currently, agents representing individuals or organizations are not being held accountable for their behavior, and this poses a risk to all stakeholders -- especially for agents' credibility. This AgentAPI is designed to help solve this dilemma.  

Specifications for this API are derived from:

HL7.org https://hl7.org/FHIR

VGS API https://www.verygoodsecurity.com/docs/api/1

The AgentAPI is organized as follows:

FHIR Message Schemas
---------------
These schemas conform to FHIR 4.01, for transporting message payload schemas according to the Implementation Guide.
A number of different schemas can be used depending on the type: REST, Messaging, Document, and System.

Simple API Bundle.xml             - lightweight request-response using <any> node for custom message payloads processed by external systems.

Simple API Search Bundle.xml      - light-weight request-response using search parameters for finding custom message payloads.

Simple MessageHeader Bundle.xml   - light-weight request-response using asynchronous messaging header schemas for containing custom message payloads.

Simple Messaging Bundle.xml       - medium-weight request-response using one or more messaging definition schemas for containing custom message payloads.

Composition Document Bundle.xml   - heavy-weight Document with multiple Composition.Sections containing custom message payloads inline or by reference.


Message Payload Schemas
---------------

  GenericRequest            - Used for testing requests without a defined payload
  
  InquiryRequest            - Requests answers to simple questions about policies and practices
  
  AccessRequest             - Requests disclosure of personal data collected
  
  RectifyRequest            - Requests correction of personal data collected
  
  RestrictProcessingRequest - Requests restrictions on processing personal data (typically with third-parties)
  
  UnsubscribeRequest        - Requests unsubscribe from all or specific subscriptions
  
  OptOutRequest             - Requests opting out from sharing, transfer, or sale of personal data
  
  DeleteRequest             - Requests deletion of personal data, and/or online accounts
  
Each of these schemas contain a request and a response message.
Each message schema can be customized according to the specific legal standard(s).
Each message schema has a variety of protocol implementations to suit most any environment.
Each message schema is based on FHIR 4.0.0.1 standards.

Enterprise Server
-----------------
  This is a proxy for an internal server that serves as a gateway to other servers.
  One required artifact is a Capability Statement, which can be queried to discover which methods and implementation requirements exist for supporting the AgentAPI.
  Optional artifacts include reference links to supporting services.
  
Consumer PII-Vault
-----------------
  This is a proxy for a consumer's repository of personal information.
  Initially it is used to store test data.
  Ultimately, it is used to store or access real personal information from one or more data sources.
  This is intended to share mostly non-sensitive personal data typically used for identity verification, contact information, and account creation. 
  
Consumer Auditor
----------------
  This component registers event-listeners for a variety of data sources, 
  which can be configured to generate log entries and notifications on specific events.
  
HTTP API Server
---------------
This is a proxy for the server hosting the API for submitting privacy rights requests 
and receiving correlated responses. 

Identity Provider
-----------------
This is a proxy for integrating with a number of identity providers using SAML, and/or oAuth.

Vendor Risk Manager
-------------------
This is a proxy for collecting evidence about vendors, running tests and publishing test results.
It also provides risk assessments and tools for vendor life-cycle management.

HTTP API Client SDKs
--------------------
These SDKs will assist developers in consuming API services and orchestrating them to support custom business processes and integration with local resources.

eDiscovery Tools
----------------
A suite of localized tools for discovery of vendors, accounts, services on personal devices and resources,
or online resources. 

Supporting Environment
----------------------
The AgentAPI design includes support for integration with other APIs and external security and compliance controls. I chose the Vault API and Control API from VGS, (Very Good Security), a firm which provides data protection solutions for PCI-DSS. 

Like FHIR, the VGS Vault also protects sensitive information, such as payment data, personally identifiable information (PII), passwords, private keys, environment variables, and SIEM data. VGS claims their APIs can reduce your compliance certification tasks by over 90% for PCI and SOC2, as well as compliance with HIPAA and GDPR. The VGS Vault is an external Consumer PII-Vault used to test AgentAPI integration with other vendors.
 
VGS also provides a Control API, which contains everything you need to either build a security program from scratch or achieve any compliance your business needs. Control fundamentally accelerates all aspects of achieving compliance by providing:

    A complete set of security controls for your organization to adopt and modify as you need
    Up to date and prescriptive tasks translated into english for non-specialists and engineers
    Automated evidence collection, saving time from screenshots and deciphering what to show to meet a control
    Ongoing real security monitoring, above and beyond compliance minimums
    The ability to assign tasks and notifications for completion
    Automated compliance headaches like vendor risk questionnaires, onboarding, and gap assessments.
    Real-time auditor interaction and evidence feedback

Control is fundamentally designed to save your team two hires: a compliance expert who has to function as a project manager, and a security engineer who has to function as an IT operations manager. By providing automated evidence and the complete audit in the tool, Control keeps your audit on track while monitoring your evidence and providing guidance.

The Control API is needed on the assumption that no system can reliably monitor and regulate itself.

Getting Started
---------------
Read the FHIR IMPLEMENTATION GUIDELINE FOR DIGITAL RIGHTS in the docs folder of this repo.
Be aware that resources exist to save time and expense of building the environment from scratch.
Google Cloud offers https://cloud.google.com/healthcare/docs/concepts/fhir.
Microsoft Azure offers https://github.com/microsoft/fhir-server.
Digital rights interchanges can also be simplified for minimally-functioning environments,
and schemas can be relaxed enough to accommodate message payloads from other systems and APIs. 

Other documents include:
  
Methodology.md - explains the steps I took in developing this AgentAPI.

Use-case scenarios.md - narratives for common use cases.

DevelopmentRoadmap.md - documents what is complete, what is planned, and what is proposed for AgentAPI.
  
DR-GCP-FHIR Implementation Guide.md - GOOGLE CLOUD PLATFORM - FHIR IMPLEMENTATION GUIDELINE FOR DIGITAL RIGHTS.
