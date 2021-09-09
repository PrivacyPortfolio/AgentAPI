# Use-Case Scenarios for AgentAPI

## Registration
 Register - Agent registers with enforcement authorities and is enrolled to AgentRegistry.
 
 Register - dataSubject registers with Agent and receives link to Portal Account and API keys.
 
 Register - Organization registers as Business, Vendor, Provider, Third-Party, etc. with Agent and receives link to Portal Account and API keys.
 
 Register - dataSubject registers Organization as IdentityProvider, and provides link to Account and API keys.
 
 Register - dataSubject registers Organization as PII-VaultProvider, and provides link to Account and API keys.


## Authorization
 Authorize - dataSubject authorizes Agent.
 
 Authorize - Organization authorizes Agent.
 
 Authorize - RelatedPerson authorizes Agent.
 
 Authorize - dataSubject authorizes Organization as IdentityProvider.
 
 Authorize - dataSubject authorizes Organization as PII-VaultProvider.
 

## Roles
 GetRole - by Actor, by Evidence, by Request.
 
 CreateRole - option on lifeChange event or when Get Role returns null.
 
 ProvisionRole - add role to list used for tagging data collection or digital rights request event.
 
 DeleteRole - remove role from list.


## Profiles
 CreateProfile - Agent generates new default Profile by template for each Organization type: (Vendor, Third-Party, etc.). Optional parameters to override or augment template.
 
 ProvisionProfile - Agent provisions dataSubject Profile.
 
 ProvisionProfile - dataSubject provisions IdentityProvider Profile.
 
 GetProfile - by Organization type or id.
 
 TestProfile - Agent tests dataSubject Profile against Conformance rules.


## Identity Management
 VerifyIdentity - Agent verifies identity of dataSubject.
 
 GetMatch - Organization requests match for dataSubject's declared datapoints.
 

## Vendor Management
 Discover - dataSubject runs Vendor Discovery tool.
 
 SelectVendor - dataSubject selects Vendors for Risk Management Program.
 
 ProvisionProfile - Agent provisions Vendor Profiles.
 
 RunTests - Agent runs Vendor baseline tests.
 
 Publish - Agent publishes Vendor baseline test results.
 
 RunTests - Agent performs entity resolution checks on Vendors.
 
 AssessRisk - Agent performs initial Vendor Risk Assessments.
 
 Publish - Agent publishes Risk Assessments.
 
 AssessRights - Agent performs Digital Rights Assessment.
 
 SelectVendor - dataSubject select Vendors for Access Requests.
 
 Generate - Agent generates Access Requests.
 
 Send - Agent sends Access Requests per Vendor policy.
 
 GenerateAndSend - Agent generates and send Inquiries to Vendors lacking policy.
 
 Listen - Agent listens for Vendor responses.


## Enforcement
 SelectVendor - dataSubject selects Vendors for termination or prosecution.
 
 Submit - Agent generates and submits complaints/referrals to enforcement authorities.
 
 Listen - Agent listens for enforcement authority responses.
 
 Publish - Agent publishes complaints/referrals to data catalog.


## Shopping
 Discover - Agent finds replacements for terminated Vendors.
 
 GenerateAndSend - Agent generates and sends Inquiries to prospective Vendors.
 
 Listen - Agent listens for prospective Vendor responses.
 
 Publish - Agent publishes prospective Vendor report to account portal.
 
 SelectVendor - dataSubject selects new Vendors.


## Purchasing
 Register - Agent registers new Vendor account portals on dataSubject's behalf.
 
 Submit - Agent populates billing and contact information required by new Vendor.
 
 Listen - Agent listens for billing notification events. 
 
 GenerateAndSend - Agent generates and sends introductory messages to Vendor contacts.

