{::options toc_levels="1..4"/}

### Introduction
There are a number of FHIR resource types used to record provider directory entries.

[PractitionerRole](StructureDefinition-au-pd-practitionerrole.html) and [HealthcareService](StructureDefinition-au-pd-healthcareservice.html) are the main searchable concepts and can be related to other resources, [Practitioner](StructureDefinition-au-pd-practitioner.html), [Organization](StructureDefinition-au-pd-organisation.html), [Location](StructureDefinition-au-pd-location.html) and [Endpoint](StructureDefinition-au-pd-sm-endpoint.html).

Searching for suitable [PractitionerRole](StructureDefinition-au-pd-practitionerrole.html) or [HealthcareService](StructureDefinition-au-pd-healthcareservice.html) can be made by search parameters on these resource types and/or via chained searches to the related resource types.

In this usage the [PractitionerRole](StructureDefinition-au-pd-practitionerrole.html) and [HealthcareService](StructureDefinition-au-pd-healthcareservice.html) resources are related to Endpoints (containing service connection details, such as secure messaging) and this allows communications to be made to a practitioner (in a role) or healthcare service based on the content of the associated Endpoint resource used.

<br/>
<br/>

### Provider Directory Service Roles

This implementation guide is defined to allow multiple consumer systems to call multiple provider directory services reliably with a consistent interface and available data support.  

#### Provider Directory Supplier

Provider Directory Supplier implementations are server software systems that supply a provider directory service interface and data.

```
SRV01 Servers SHALL be capable of providing all resource types included in this guide.
```

```
SRV02 Servers SHALL be capable of providing all profile data elements marked as MUST SUPPORT.
```

```
SRV03 Servers SHALL comply to AU Provider Directory Implementation Guide, AU PD Implementation Guide and FHIR R4 core constraints for all resource instances.
```

```
SRV04 Servers SHALL be capable of responding meaningfully to all search requests (in each resource definition) that are marked as MUST SUPPORT.
```

```
SRV05 Servers MAY be capable of responding to search requests (in each resource definition) that are marked as OPTIONAL.
```

```
SRV06 Servers MAY be capable of responding to other search requests that are FHIR core compliant OR custom searches (defined using CapabilityStatement supplied by the server).
```
<br/>

#### Provider Directory Consumer 

Provider Directory Consumer implementations are client software systems that call provider directory services and consume data.

```
CLI01 Clients SHALL support meaningful consumption of all data elements marked as MUST SUPPORT.
```

```
CLI02 Clients SHALL allow receipt of the all resource types in this guide.
```

```
CLI03 Clients SHALL allow receipt of resource instances that are valid according to the AU Provider Directory Implementation Guide, AU PD Implementation Guide and FHIR R4 core compliant elements not defined in this guide.
```

```
CLI04 Clients can assume that all search requests marked as MUST SUPPORT are available.
```

```
CLI05 Clients may use search requests marked as OPTIONAL but MUST inform the user if the call is not supported by the server.
```

<br/>
<br/>


### Provider Directory Core Entity Relationships

For directory service profiles, the [HealthcareService](StructureDefinition-au-pd-healthcareservice.html) is used to represent a practice or clinic as they would be externally advertised in a directory. The [HealthcareService](StructureDefinition-au-pd-healthcareservice.html) is delivering a category of services at a [Location](StructureDefinition-au-pd-location.html) by an [Organization](StructureDefinition-au-pd-organisation.html).

The [PractitionerRole](StructureDefinition-au-pd-practitionerrole.html) is used to represent a [Practitioner](StructureDefinition-au-pd-practitioner.html) practicing at those practices or clinics where they are externally advertised.

<div>
<img src="{{site.baseurl}}erd3.png"/>
</div>
<br/>
<br/>

#### Relationship guidance
Given the implementation guide allows Endpoints to be referenced from PractitionerRoles, HealthcareServices and Locations, clarity is required as to what it means for an Endpoint to be referenced from each of these entities, where a client should look for an endpoint and the supported payloads:

```
REL01 If a PractitionerRole is reachable through an Endpoint, the PractitionerRole MUST explicitly reference that Endpoint.
```

```
REL02 Clients wishing to address a message to a PractitionerRole MUST use an Endpoint directly referenced by that PractitionerRole.
```

```
REL03 Clients wishing to address a message to a HealthcareService MUST use an Endpoint directly referenced by that HealthcareService.
```

```
REL04 Clients cannot assume that a PractitionerRole is reachable through an Endpoint referenced by the HealthcareService – unless the Endpoint is also directly referenced by the PractitionerRole.
```

```
REL05 Endpoints referenced from a Location are used to communicate with the Location itself (e.g. the building management), and not the HealthcareService or PractitionerRole at that Location. For this reason, clients should not use Endpoint references on Locations.
```

```
REL06 The binding to the valueset for Australian Endpoint Payload Types is extensible.  Implementers should use payload types defined in the valueset where possible. Where this is not possible, implementers may reach local agreements to use payload types that are not listed.
```

```
REL07 Hours of operation at a Location are for the Location itself rather than the times any HealthcareService or PractitionerRole is available. Directory clients should be using the HealthcareService or PractitionerRole available times.
```

<br/>
<br/>

### Provider Directory Usage Sequence for Secure Messaging

Typical sequence describing endpoint search, HL7 V2 generation, secure message composition, secure message delivery via intermediary, acknowledgement response addressing, generation and delivery.
	
<div>
<img src="{{site.baseurl}}sequence2.png" width="1200" height="800"/>
</div>
<br/>
<br/>

#### Component Roles
1. USER
* User of the PMS/CIS (practice management system/clinical information system) sending a message.
1. PMS/CIS SENDER
* The practice management system or clinical information system) formatting and sending a HL7 V2 message.
1. SM CLIENT (SENDER)
* Secure messaging client component/system that can deliver payloads using secure messaging standard [ATS 5822—2010 — E-Health Secure Message Delivery](https://infostore.saiglobal.com/en-au/Standards/Product-Details-129644_SAIG_AS_AS_274353/?productID=129644_SAIG_AS_AS_274353).
1. SM INTERMEDIARY
* Secure messaging intermediary providing store and forward .
* May provide routing to other secure messaging suppliers.
1. PD FHIR SERVER
* Provider directory FHIR API service adhering to this implementation guide.
* May be federated across multiple provider directory suppliers to allow addressing suitable for forwarding to other secure messaging suppliers.
1. SM CLIENT (RECEIVER)
* Secure messaging client component/system that can receive payloads using secure messaging standard [ATS 5822—2010 — E-Health Secure Message Delivery](https://infostore.saiglobal.com/en-au/Standards/Product-Details-129644_SAIG_AS_AS_274353/?productID=129644_SAIG_AS_AS_274353).
1. PMS/CIS (RECEIVER)
* The practice management system or clinical information system) receiving the HL7 V2 message and processing it.
* On accept/reject of the received message formatting and sending a HL7 V2 ACK message.

#### Sequence
1. USER SEARCH
* User performs a provider search in there client application.
* This will likely include search parameters to use on search calls.
1. SEARCH (1)
* Search on PractitionerRole and/or HealthcareService (and related resources) can be done on PD FHIR Server.
* Available search parameters are defined in Quick Start section of each profile which may be combined as the client application supports.
* For example search by active status, secure messaging support, payload support and provider number.
```
GET https://sqlonfhir-aupd.azurewebsites.net/fhir/PractitionerRole?active=true&identifier=http://ns.electronichealth.net.au/id/medicare-provider-number|2426621B&endpoint.connection-type=http://hl7.org.au/fhir/pd/CodeSystem/smd-interfaces|http://ns.electronichealth.net.au/smd/intf/SealedMessageDelivery/TLS/2010&endpoint.payload-type=http://hl7.org.au/fhir/pd/CodeSystem/endpoint-payload-type|http://ns.hl7.org.au/hl7v2/profiles/HL7AU-OO-REF-SIMPLIFIED-201706-L1&_include=PractitionerRole:endpoint&_include=PractitionerRole:location&_include=PractitionerRole:organization&_include=PractitionerRole:practitioner
```
* For example search by active status, secure messaging support, payload support and practitioner family name
```
GET https://sqlonfhir-aupd.azurewebsites.net/fhir/PractitionerRole?active=true&practitioner.family=Smith&endpoint.connection-type=http://hl7.org.au/fhir/pd/CodeSystem/smd-interfaces|http://ns.electronichealth.net.au/smd/intf/SealedMessageDelivery/TLS/2010&endpoint.payload-type=http://hl7.org.au/fhir/pd/CodeSystem/endpoint-payload-type|http://ns.hl7.org.au/hl7v2/profiles/HL7AU-OO-REF-SIMPLIFIED-201706-L1&_include=PractitionerRole:endpoint&_include=PractitionerRole:location&_include=PractitionerRole:organization&_include=PractitionerRole:practitioner
```
* For example search by active status, secure messaging support, payload support and individual specialty in postcode
```
GET https://sqlonfhir-aupd.azurewebsites.net/fhir/PractitionerRole?active=true&location.address-postalcode=3010&specialty=http://snomed.info/sct|17561000&endpoint.connection-type=http://hl7.org.au/fhir/pd/CodeSystem/smd-interfaces|http://ns.electronichealth.net.au/smd/intf/SealedMessageDelivery/TLS/2010&endpoint.payload-type=http://hl7.org.au/fhir/pd/CodeSystem/endpoint-payload-type|http://ns.hl7.org.au/hl7v2/profiles/HL7AU-OO-REF-SIMPLIFIED-201706-L1&_include=PractitionerRole:endpoint&_include=PractitionerRole:location&_include=PractitionerRole:organization&_include=PractitionerRole:practitioner
```
* Can perform an independent Endpoint lookup based on results or a direct lookup as below.
* Example lookup endpoint directly by the secure messaging target identifier directly - vendor or other target (perhaps stored in local directory)
```
http://sqlonfhir-aupd.azurewebsites.net/fhir/Endpoint?status=active&identifier=http://ns.electronichealth.net.au/smd/target|http://ns.argusdca.com.au/smd/id/hostname/ACC5408570000002&connection-type=http://hl7.org.au/fhir/pd/CodeSystem/smd-interfaces|http://ns.electronichealth.net.au/smd/intf/SealedMessageDelivery/TLS/2010&payload-type=http://hl7.org.au/fhir/pd/CodeSystem/endpoint-payload-type|http://ns.hl7.org.au/hl7v2/profiles/HL7AU-OO-REF-SIMPLIFIED-201706-L1
```
1. SEARCH RESPONSE (1)
* A FHIR Bundle (searchset) is returned as the search result.
* [Example a PractitionerRole search response](Bundle-search1.html) with one PractitionerRole and included resources returned.
* [Example a HealthcareService search response](Bundle-search2.html) with one HealthcareService and included resources returned. 
* Depending on search made on PD FHIR Server this will likely contain details:
  * PractitionerRole, Practitioner, Location, Organization, Endpoint.
  * HealthcareService, Location, Organization, Endpoint.
1. USER SELECT
* Details of the results received are presented to the user.
* The user can select the desired receiver as needed.
1. GENERATE HL7 V2 MESSAGE
* The [compliant HL7 V2 payload](ValueSet-endpoint-payload-type.html) needs to be generated.
* Utilise Endpoint result content for generating HL7 V2 content.
  * Endpoint au-receivingfacility extension (HL7 V2 Receiving Facility)
  * Endpoint au-receivingapplication extension (HL7 V2 Receiving Application)
* Use the [Australian Diagnostics and Referral Messaging](https://confluence.hl7australia.com/display/OO/Australian+Diagnostics+and+Referral+Messaging+-+Localisation+of+HL7+Version+2.4) guidance for HL7 V2 formatting:
  * Fill HL7 V2 MSH-4, MSH-6 content as per [Australian Diagnostics and Referral Messaging - Facility/Organisational level addressing](https://confluence.hl7australia.com/pages/viewpage.action?pageId=31589320#Appendix10AddressingmessagesusingAustralianProfileforProviderDirectoryServices(Normative)-A10.1.1Facility/Organisationalleveladdressing).
  * Fill HL7 V2 PRD-1, PRD-2, PRD-3, PRD-5, PRD-7 and/or XCN datatype content as per [Australian Diagnostics and Referral Messaging - Intended Provider/Individual recipient level addressing](https://confluence.hl7australia.com/pages/viewpage.action?pageId=31589320#Appendix10AddressingmessagesusingAustralianProfileforProviderDirectoryServices(Normative)-A10.1.2IntendedProvider/Individualrecipientleveladdressing).
  * Fill HL7 V2 MSH-3, MSH-5 content as per [Australian Diagnostics and Referral Messaging - Application level addressing](https://confluence.hl7australia.com/pages/viewpage.action?pageId=31589320#Appendix10AddressingmessagesusingAustralianProfileforProviderDirectoryServices(Normative)-A10.1.3Applicationleveladdressing).
1. SEND HL7 V2 Message
* Drop outbound HL7 V2 file or send via local SM CLIENT API.
1. SEARCH (2) 
* For independent HL7 V2 sender processing (no Endpoint knowledge) can lookup Endpoint details via search by Receiving Facility.
* Examples - lookup Endpoint by HL7 V2 content using [AU PD search parameter extensions](search-parameters.html) for delivery via secure messaging.
```
GET https://jdfhir.test.medical-objects.com.au/rest/fhir/Endpoint?status=active&au-receivingfacility-namespace-id=8003623233355378&au-receivingfacility-universal-id=1.2.36.1.2001.1003.0.8003623233355378&au-receivingfacility-universal-id-type=ISO&connection-type=http://hl7.org.au/fhir/pd/CodeSystem/smd-interfaces|http://ns.electronichealth.net.au/smd/intf/SealedMessageDelivery/TLS/2010&payload-type=http://hl7.org.au/fhir/pd/CodeSystem/endpoint-payload-type|http://ns.electronichealth.net.au/ack/sc/deliver/hl7Ack/2012 
```
```
GET http://sqlonfhir-aupd.azurewebsites.net/fhir/Endpoint?status=active&au-receivingfacility-namespace-id=CIB&au-receivingfacility-universal-id=877F9695-1298-4E6A-B432-0FDD46AD80B8&au-receivingfacility-universal-id-type=GUID&connection-type=http://hl7.org.au/fhir/pd/CodeSystem/smd-interfaces|http://ns.electronichealth.net.au/smd/intf/SealedMessageDelivery/TLS/2010&payload-type=http://hl7.org.au/fhir/pd/CodeSystem/endpoint-payload-type|http://ns.electronichealth.net.au/ack/sc/deliver/hl7Ack/2012
```
1. SEARCH RESPONSE (2)
* A FHIR Bundle (searchset) is returned with Endpoint details needed to deliver HL7 V2 Message
* [Example an Endpoint search response](Bundle-search3.html) with one Endpoint resource returned.
1. COMPOSE SECURE MESSAGE (1)
* Use details in the Endpoint result to compose standard SMD messages [ATS 5822—2010 — E-Health Secure Message Delivery](https://infostore.saiglobal.com/en-au/Standards/Product-Details-129644_SAIG_AS_AS_274353/?productID=129644_SAIG_AS_AS_274353).
* Utilise Endpoint result content for composing message including:
  * Endpoint.identifier (Secure Messaging Target Identifier) - destination reference (to SMD receiverOrganisation element).
  * Endpoint encryption-certificate-pem-x509 extension (PEM X509 Certificate) - encrypting certificate (for SMD encryptedPayload production).
  * Endpoint.payloadType - type of message payload (to SMD serviceCategory element).
  * Endpoint.contentType - method of interfacing (to SMD serviceInterface element).
1. DELIVER (1)
* Use details in the Endpoint result to deliver secure messages [ATS 5822—2010 — E-Health Secure Message Delivery](https://infostore.saiglobal.com/en-au/Standards/Product-Details-129644_SAIG_AS_AS_274353/?productID=129644_SAIG_AS_AS_274353).
* Utilise Endpoint result content for composing message including:
  * Endpoint.address - URL endpoint addressing.
1. RETRIEVE (1)
* Retrieve secure messages as per standard [ATS 5822—2010 — E-Health Secure Message Delivery](https://infostore.saiglobal.com/en-au/Standards/Product-Details-129644_SAIG_AS_AS_274353/?productID=129644_SAIG_AS_AS_274353).
1. IMPORT HL7 V2 MESSAGE
* Import HL7 V2 Message for PMS/CIS application level processing (accept/reject).
1. SEARCH (3)
* For independent HL7 V2 receiver processing can use the inbound HL7 V2 content (MSH Sending Facility) to perform a lookup to obtain path for HL7 V2 ACK message delivery
* Examples - lookup Endpoint by HL7 V2 content using [AU PD search parameter extensions](search-parameters.html) for ACK delivery via secure messaging
```
GET https://jdfhir.test.medical-objects.com.au/rest/fhir/Endpoint?status=active&au-receivingfacility-namespace-id=8003623233355378&au-receivingfacility-universal-id=1.2.36.1.2001.1003.0.8003623233355378&au-receivingfacility-universal-id-type=ISO&connection-type=http://hl7.org.au/fhir/pd/CodeSystem/smd-interfaces|http://ns.electronichealth.net.au/smd/intf/SealedMessageDelivery/TLS/2010&payload-type=http://hl7.org.au/fhir/pd/CodeSystem/endpoint-payload-type|http://ns.electronichealth.net.au/ack/sc/deliver/hl7Ack/2012 
```
```
GET http://sqlonfhir-aupd.azurewebsites.net/fhir/Endpoint?status=active&au-receivingfacility-namespace-id=CIB&au-receivingfacility-universal-id=877F9695-1298-4E6A-B432-0FDD46AD80B8&au-receivingfacility-universal-id-type=GUID&connection-type=http://hl7.org.au/fhir/pd/CodeSystem/smd-interfaces|http://ns.electronichealth.net.au/smd/intf/SealedMessageDelivery/TLS/2010&payload-type=http://hl7.org.au/fhir/pd/CodeSystem/endpoint-payload-type|http://ns.electronichealth.net.au/ack/sc/deliver/hl7Ack/2012
```
1. SEARCH RESPONSE (3)
* A FHIR Bundle (searchset) is returned with Endpoint details needed to deliver ACK.
* [Example an Endpoint search response](Bundle-search3.html) with one Endpoint resource returned.
* Use information to format the HL7 V2 ACK message.
1. GENERATE HL7 V2 ACK
* Use same method to format HL7 V2 ACK as GENERATE HL7 V2 MESSAGE.
1. SEND HL7 V2 ACK
* Drop outbound HL7 V2 ACK file or send via local SM CLIENT API.
1. SEARCH (4)
* For independent HL7 V2 ACK send processing can use the outbound HL7 V2 content (MSH Receiving Facility) to perform a lookup to obtain path for HL7 V2 ACK message delivery
* Examples - lookup Endpoint by HL7 V2 content using [AU PD search parameter extensions](search-parameters.html) for ACK delivery via secure messaging
```
GET https://jdfhir.test.medical-objects.com.au/rest/fhir/Endpoint?status=active&au-receivingfacility-namespace-id=8003623233355378&au-receivingfacility-universal-id=1.2.36.1.2001.1003.0.8003623233355378&au-receivingfacility-universal-id-type=ISO&connection-type=http://hl7.org.au/fhir/pd/CodeSystem/smd-interfaces|http://ns.electronichealth.net.au/smd/intf/SealedMessageDelivery/TLS/2010&payload-type=http://hl7.org.au/fhir/pd/CodeSystem/endpoint-payload-type|http://ns.electronichealth.net.au/ack/sc/deliver/hl7Ack/2012 
```
```
GET http://sqlonfhir-aupd.azurewebsites.net/fhir/Endpoint?status=active&au-receivingfacility-namespace-id=CIB&au-receivingfacility-universal-id=877F9695-1298-4E6A-B432-0FDD46AD80B8&au-receivingfacility-universal-id-type=GUID&connection-type=http://hl7.org.au/fhir/pd/CodeSystem/smd-interfaces|http://ns.electronichealth.net.au/smd/intf/SealedMessageDelivery/TLS/2010&payload-type=http://hl7.org.au/fhir/pd/CodeSystem/endpoint-payload-type|http://ns.electronichealth.net.au/ack/sc/deliver/hl7Ack/2012
```
* Example - if known lookup endpoint by the secure messaging target identifier - vendor or other target for ACK message delivery via secure messaging
```
http://sqlonfhir-aupd.azurewebsites.net/fhir/Endpoint?status=active&identifier=http://ns.electronichealth.net.au/smd/target|http://ns.argusdca.com.au/smd/id/hostname/ACC5408570000002&connection-type=http://hl7.org.au/fhir/pd/CodeSystem/smd-interfaces|http://ns.electronichealth.net.au/smd/intf/SealedMessageDelivery/TLS/2010&payload-type=http://hl7.org.au/fhir/pd/CodeSystem/endpoint-payload-type|http://ns.electronichealth.net.au/ack/sc/deliver/hl7Ack/2012
```
1. SEARCH RESPONSE (4)
* A FHIR Bundle (searchset) is returned with Endpoint details needed to deliver ACK
* [Example an Endpoint search response](Bundle-search3.html) with one Endpoint resource returned.
* Use information to compose a secure message containing ACK.
1. COMPOSE SECURE MESSAGE (2)
* Use same method to compose ACK secure message as COMPOSE SECURE MESSAGE (1).
1. DELIVER (2)
* Use same method to compose ACK secure message as DELIVER (1).
1. RETRIEVE (2)
* Standard retrieval of secure message HL7 V2 ACK response as per RETRIEVE (1).
1. IMPORT HL7 V2 ACK
* Import HL7 V2 ACK for PMS/CIS application processing.

AU PD defines the data model and RESTful API interactions that set minimum expectations for a system to record, update, search, and retrieve Core digital health and administrative information.

The approach to describing the 'minimum' in AU PD means modelling in such a way that 'at least' what is to be supported is defined without limiting meaningful options for use cases and business rules. AU PD artefacts are therefore modelled as open templates allowing for additional content including elements, extensions, resources, search parameters, operations, and terminology whilst ensuring the minimum requirements are met.

Australian realm IGs and implementers are expected to comply with AU Base and AU Core and to define extensions, search parameters or operations (in order of precedence):
* in the FHIR standard (incl. FHIR Extensions Pack)
* in AU Base
* in AU Core
* in a local IG only if necessary

#### Scope of AU PD FHIR Artefacts

AU PD profiles:
* [Extensions](general-guidance.html#extension-approach) to define minimum support expectations for complex extensions.
* [Search parameters](general-guidance.html#search-parameter-approach) to define minimum support expectations (e.g. support for chaining) for supported search parameters.
* [Resources](general-guidance.html#resource-profile-approach) to define minimum support expectations for the use of FHIR resources.

AU PD defines:
* [Actors](general-guidance.html#actor-approach) to define systems that play a role in AU PD data exchange.
* [Capability statements](general-guidance.html#capability-statement-approach) to define the minimum capabilities (behaviours) to be supported for AU PD actors.

AU PD does not define new extensions, search parameters, operations or [terminology](general-guidance.html#terminology-approach); these artefacts are defined in AU Base. AU PD profiles these AU Base artefacts to describe minimum expectations for support in Australia. For more information on the definitional nature of AU Base see the guidance in [AU Base Implementation Guide Approach](https://build.fhir.org/ig/hl7au/au-fhir-base/generalguidance.html#implementation-guide-approach).

#### Extension Approach
AU PD does not define new extensions. All extensions included in AU PD are defined in the [FHIR Extensions Pack](https://hl7.org/fhir/extensions/) or [AU Base](http://build.fhir.org/ig/hl7au/au-fhir-base/profiles-and-extensions.html#extensions). A limited set of extensions are indicated as _Must Support_ in AU PD resource profiles (see [Use of Extensions](general-guidance.html#use-of-extensions) in [Resource Profile Approach](general-guidance.html#resource-profile-approach)).

AU PD profiles some complex extensions to define minimum support expectations, for example [AU Core Sex Assigned At Birth](https://build.fhir.org/ig/hl7au/au-fhir-core/StructureDefinition-au-core-rsg-sexassignedab.html) profiles the [Person Recorded Sex or Gender extension](https://hl7.org/fhir/extensions/StructureDefinition-individual-recordedSexOrGender.html) to indicate which parts of the extension are _Must Support_ and the obligations for AU Core actors.

When modelling AU PD extension profiles, the [AU PD resource profile approach](general-guidance.html#resource-profile-approach) is followed.

#### Search Parameter Approach
AU PD does not define new search parameters. All search parameters included in AU PD are defined in the [FHIR Search Parameter Registry](https://hl7.org/fhir/R4/searchparameter-registry.html) or [AU Base](http://build.fhir.org/ig/hl7au/au-fhir-base/search-parameters.html).

AU PD profiles these search parameters to describe the additional minimum support expectations for search parameters for AU PD actors including support for chaining, modifiers, and comparators, e.g. [AU PD profile of practitionerrole-practitioner](https://build.fhir.org/ig/hl7au/au-fhir-core/SearchParameter-au-core-practitionerrole-practitioner.html) that defines support for chained identifiers.

<div class="stu-note" markdown="1">
The set of AU PD SearchParameters defined in this release of AU PD include search parameters with support for chained search. Future releases of AU PD will define the complete list of search parameters supported by AU PD.
</div>

#### Terminology Approach
AU PD does not define new terminology FHIR artefacts (e.g. value sets or code systems). Terminology supported in AU PD are published in [AU Base](https://build.fhir.org/ig/hl7au/au-fhir-base/terminology.html), the FHIR standard, [HL7 Terminology (THO)](https://terminology.hl7.org/), or the [National Clinical Terminology Service (NCTS)](https://www.healthterminologies.gov.au/). 
AU PD does not define new terminology FHIR artefacts (e.g. value sets or code systems). Terminology supported in AU PD are published in [AU Base](https://build.fhir.org/ig/hl7au/au-fhir-base/terminology.html), the FHIR standard, [HL7 Terminology (THO)](https://terminology.hl7.org/), or the [National Clinical Terminology Service (NCTS)](https://www.healthterminologies.gov.au/). 

#### Resource Profile Approach
AU PD resource profiles set minimum expectations for a system to record, update, search, and retrieve core digital health and administrative information in Australia. AU PD profiles are derived from AU Base resource profiles, where available, and identify the additional mandatory core elements, extensions, vocabularies and value sets that are to be present in the resource when conforming to AU PD. These profiles define the minimum conformance requirements that downstream profiles are expected to comply with.

AU PD profiles:
* are derived from AU PD, where available, to inherit the nationally agreed localised terminology, identifiers, and extensions.
* are only constrained (e.g. cardinality) where the constraint is agreed to be a minimum requirement that is nationally relevant and applicable across use cases to avoid limiting options for downstream use case decisions and business rules. 
* elements and extensions that form the minimum requirements are labelled _Must Support_. 
* elements that are not labelled _Must Support_ are not part of the minimum requirements and are therefore not further constrained in AU PD.
* are defined as open, allowing additional elements, extensions, and rules. This results in a more flexible template that can be used across wider contexts, but also means that the resource content is not closed, and applications have to decide how to handle content not described by the profile.  

<!-- Some aspects of the approach to [profiling](https://hl7.org/fhir/R4/profiling.html) resources in AU Core are described in further detail in the subsections below:
* [Restricting Cardinality](general-guidance.html#restricting-cardinality)
* [Use of Extensions](general-guidance.html#use-of-extensions)
* [Use of Terminology Bindings](general-guidance.html#use-of-terminology-bindings)
* [Use of Invariants](general-guidance.html#use-of-invariants)
* [Use of Slicing, Pattern, and Fixed Value ](general-guidance.html#use-of-slicing-pattern-and-fixed-value)
* [Restricting References and Type Choices](general-guidance.html#restricting-references-and-type-choices)
* [Use of Must Support and Obligations](general-guidance.html#use-of-must-support-and-obligations) -->

AU PD does not prohibit (cardinality 0..0) elements or extensions, as prohibition restricts the opportunities for downstream IGs and applications to define their own business rules.

##### Use of Extensions

Extensions are inherited from the underlying AU Core resource profile, where available, and the extensions that form part of the minimum support expectations for an AU PD profile are marked with _Must Support_. 

Additional extensions are not added directly to an AU PD profile, unless there is no underlying AU Base resource profile to derive from. Some FHIR resource types are not profiled in AU Base as the resource type is too abstract to support meaningful localisation across use cases in a base resource profile (e.g. Basic, Observation, or Device).

While the work to include a new extension in the underlying AU Base resource profile is progressing, extensions can be added (but not defined) temporarily in an AU PD profile to support development and testing in a release.

##### Use of Must Support and Obligations
_[Must Support](general-requirements.html#must-support-and-obligation)_ is used to indicate the elements and extensions that form the minimum requirements of AU PD. Labelling an element _Must Support_ means that systems that produce or consume resources are to provide support for the element in some meaningful way. The FHIR standard does not define exactly what 'meaningful' support for an element means, but indicates that a profile needs to make clear exactly what kind of support is required when an element is labelled as _Must Support_.

When defining the meaning of _Must Support_ in AU PD:
* [Profile Only Support](general-requirements.html#profile-only-support) is defined in narrative in the IG (e.g. [Missing Data](general-requirements.html#missing-data) requirements)
* [Profile Support + Interaction Support](general-requirements.html#profile-support--interaction-support) is defined in:
  * narrative in the IG (e.g. [Missing Data](general-requirements.html#missing-data) requirements)
  * the [obligation extension](https://hl7.org/fhir/extensions/StructureDefinition-obligation.html) on each supported element in a profile, identifying the support expectations for AU PD actors
  * narrative qualification of element obligations present in the profile specific implementation guidance

See [Must Support and Obligation](general-requirements.html#must-support-and-obligation) for a detailed description of how this is applied in AU PD. 

#### Actor Approach
AU PD actors are defined to describe the specific sets of functionality supported by systems that play a role in AU PD data exchange. Each actor is defined by:
* an actor definition that includes reference to support expectations (narrative conformance requirements and capability statements)
* a capability statement that describes the requirements for the system and resource support

#### Capability Statement Approach
AU PD capability statements are defined to describe the specific capabilities of a system when playing a particular role (i.e. actor) and includes the complete list of FHIR profiles, RESTful operations, and search parameters supported by that actor.

AU PD capability statements are defined as open, allowing additional resources, profiles, operations, and search parameters to be defined.

Every resource type that is the target of a _Must Support_ element in a profile is modelled, with the agreed support expectations (incl. base and supported profiles, operations, search parameters), to ensure a complete description of the requirements for implementation. 

AU PD capability statements include profile specific implementation guidance that provides qualifying narrative where further clarification for _Must Support_  behaviour is required or where agreed relaxations apply. 

### SNOMED CT Australian (SNOMED CT-AU) Edition
For guidance on SNOMED CT-AU in FHIR, see the guidance defined in AU Base [SNOMED CT Australian Edition](https://build.fhir.org/ig/hl7au/au-fhir-base/generalguidance.html#snomed-ct-australian-edition).

### Contained Resources
In some circumstances, the content referred to in the resource reference does not have an independent existence apart from the resource that contains it - it cannot be identified independently, and nor can it have its own independent transaction scope. For example, use of a Medication resource to represent medicinal product identification within the context of a MedicationRequest. In these circumstances the resource can be [contained](http://hl7.org/fhir/R4/references.html#contained). 

In AU PD profiles:
- Systems constructing a resource that represent medication information are encouraged to make use of contained resources within the context of a FHIR transaction. Operations on Medication resources are expected to be within the context of a referencing resource query such as an MedicationAdministration, MedicationDispense, MedicationRequest or MedicationStatement.
- If referencing a contained resource, both the contained resource and the referencing resource **SHALL** conform to the applicable AU PD profile.
- Otherwise, it is recommended that an AU PD Responder avoids the use of contained resources unless the referenced resource does not have an independent existence apart from the resource that contains it and cannot be identified independently.

Further guidance about the general use case for [contained resources](http://hl7.org/fhir/R4/references.html#contained) can be found in the FHIR standard.

### Extensibility – 'Additional' Elements
A responder can send 'additional' elements beyond those flagged with *Must Support* in an AU PD profile. Additional elements are often required by other profiles the system may conform to, allowing local requirements, including technical and workflow context for the resource, to be reflected and extending the health information supported in exchanges. For this reason, extensibility is generally encouraged and expected in AU PD profiles. Only in some exceptionally rare use case profiles are rules tightened to limit the nature of additional information that can be sent. Specification authors are encouraged to enable greater interoperability and software re-use by avoiding reductions in an element's cardinality.

Depending on local requirements, a requester (i.e. client application) may ignore these 'additional' elements, may treat the data as for rendering only, or be capable of recognising and using the element. 

### Read/Search Syntax

Searching resources is defined by the [FHIR RESTful API](https://hl7.org/fhir/R4/http.html) and included here for informative purposes. The [AU PD CapabilityStatements](capability-statements.html) document the server and client rules for the RESTful interactions described in this guide.

All the search interaction examples in this guide use the HTTP GET method with the following syntax:

 **`GET [base]/[Resource-type]?[parameter1]{:m1|m2|...}={c1|c2|...}[value1{,value2,...}]{&[parameter2]{:m1|m2|...}={c1|c2|...}[value1{,value2,...}]&...}`**

-   GET is the HTTP verb used for fetching a resource
-   Variables surrounded by "\[\]" are mandatory *for the client* to supply and will be replaced by the string literal identified.
-   Variables surrounded by "\{\}" are optional *for the client* to supply and will be replaced by the string literal identified.
-   Variables:
    -  base: The Service Root URL (e.g., "https://fhir-open-api-dstu2.smarthealthit.org”)
    -  Resource-type: The name of a resource type (e.g., "Practitioner")
    -  parameter: The search parameters as defined for the particular interaction (e.g.,"?practitioner=Practitioner/123")
    -  value: The search parameter value for a particular search
       - When searching using the `token` type SearchParameter [(how to search by token)](https://hl7.org/fhir/R4/search.html#token), the syntax `{system|}[code]` means that the system value is optional *for the client* to supply.
       - When searching using the `reference` type SearchParameter [(how to search by reference)](https://hl7.org/fhir/R4/search.html#reference), the syntax `{Type/}[id]` means that the Type value is optional *for the client* to supply.
       - When searching using the `date` type SearchParameter [(how to search by date)](https://hl7.org/fhir/R4/search.html#date), the syntax `{gt|lt|ge|le}[date]` means the date comparators "gt", "lt", "ge", and "le" are optional.   Date type searches without a comparator prefix are equivalent to searches with the "eq" comparator *even if* a server does not support the comparator.
    - \{:m1&#124;m2&#124;...}: The list of supported search parameter modifiers
    - {c1&#124;c2&#124;...}: The list of supported search parameter comparators
    - {,value2,...}: Optional multiple 'OR' Values
    - {&parameter2={:m1&#124;m2&#124;...}={c1&#124;c2&#124;...}[value1{,value2,...}&...}: Optional multiple 'AND' search parameters


In the simplest case, a search is executed by performing a GET operation in the RESTful framework:

`GET [base]/[Resource-type]?name=value&...`

For this RESTful search, the parameters are a series of name=\[value\] pairs encoded in the URL. The search parameter names are defined for each resource. For example, the Endpoint resource has the name "status" for searching on the endpoint status. For more information, see the [FHIR RESTful Search API](https://hl7.org/fhir/R4/http.html#search).

Examples in AU PD do not demonstrate the url encoding [rules for special characters](https://hl7.org/fhir/R4/search.html#escaping) e.g. "\|".