### Introduction

AU PD is provided to support the use of HL7<sup>&reg;</sup> FHIR<sup>&reg;&copy;</sup> in an Australian context. It sets the minimum expectations on FHIR resources to support conformance and implementation in systems.

AU PD defines the data model and RESTful API interactions that set minimum expectations for a system to record, update, search, and retrieve core digital health and administrative information. 

AU PD uses AU Core and AU Base representations, where available, as the basis for profiles that define the FHIR resources to be supported, along with their elements, extensions, vocabularies, and value sets, and how they are used is defined. It also documents the minimum FHIR RESTful interactions for each resource type to access data. AU PD promotes interoperability and adoption through common implementation and provides the basis for further implementation guide development for specific use cases.

There are two different ways to implement AU PD:
1. Profile Only Support: systems support only the resources as profiled by AU PD to represent digital health information.
1. Profile Support + Interaction Support: systems support the resources as profiled by AU PD to represent digital health information (Profile Support) and the [RESTful FHIR API](http://hl7.org/fhir/R4/http.html) interactions defined for it (Interaction Support).

In this regard it is similar in nature to other national core specifications such as National Healthcare Directory Exchange FHIR Implementation Guide.

For a detailed description of the different ways to implement AU PD see the [Conformance Requirements](general-requirements.html) page.

This guide covers capability requirements of FHIR services to implement a set of profiles and support interfaces in an Australian context for the purpose of implementation of provider directory services.

Many of the resource profiles provided here are based on definitions from the Australian Core Profiles Implementation Guide.

This document is a working specification that is expected to be implemented and tested by FHIR<sup>&reg;&copy;</sup> system producers to enable feedback to improve the content of this guide.

### Project Background

This implementation guide is under development through the AU PD project.

### Dependencies

{% include dependency-table.xhtml %}
{% include expansion-params.xhtml %}

### Usage

AU PD is particularly useful in defining:

- A testable level of system conformance
- Assumed support by client applications
- The basis of downstream implementation guides

Implementation of capabilities defined in AU PD enables specifications, applications and business logic to be developed with confidence.

This document is a working specification that may be directly implemented by FHIR<sup>&reg;&copy;</sup> system producers.

FHIR<sup>&reg;&copy;</sup> Connectathon events are key to the verification of the guide as being suitable for 
implementation. This implementation guide will be used as the basis for Australian Connectathon events.

### AU PD Actors

The following actors are part of AU PD:

**AU PD Requestor**

A system that creates and initiates a data access request to retrieve core digital health and administrative information. The AU PD Requestor is the client in a client-server interaction.  
<br/>

**AU PD Responder**

A system that responds to the data access request submitted by requesters, providing responses to requests for core digital health and administrative information. The AU PD Responder is the server in a client-server interaction.  
<br/>

### Scope

#### Information
* **Organisation Directory Entry**: managing organisations
* **Practitioner Directory Entry**: individual practitioners
* **Practitioner Role Directory Entry**: role participants at a location for an organisation
* **Healthcare Service Directory Entry**: services delivered at a location for an organisation
* **Secure Messaging Endpoint Directory Entry**: connection details for secure messaging

#### AU PD FHIR RESTful Interactions
For systems that support AU PD FHIR RESTful interactions ([Profile Support + Interaction Support](general-requirements.html#profile-support--interaction-support)), the interaction requirements, such as read and search operations, are detailed in the AU PD Requester and AU PD Responder [CapabilityStatements](capability-statements.html).

The following are simple examples of AU PD FHIR RESTful interactions between AU PD Requester and AU PD Responder actors:

**Example of a FHIR RESTful read interaction**

<div> 
  <img src="au-pd-read-interaction-example.svg" alt="FHIR RESTful read interaction example" style="width:60%"/>
</div>
<br/>
*Figure 1: Read a Practitioner using the logical id*
<br/>
<br/>

**Example of a FHIR RESTful search interaction**

<div> 
  <img src="au-pd-search-interaction-example.svg" alt="FHIR RESTful search interaction example" style="width:60%"/>
</div>
<br/>
*Figure 2: Search for practitioners that match a family name*
<br/>

#### Patterns of Use
* **Federated Directory** approach to supply responses based on a combination of results from multiple directory service providers
* **Synchronisation** approach to allow service supplied directory entry updates or deletion to be systematically synchronised with a local directory

### Usage

This document is a working specification that may be directly implemented by FHIR<sup>&reg;&copy;</sup> system producers.

Information contained in this document provides a minimal requirement level for use in secure message end point location and addressing. 
To that end it specifies key entities and elements thereof needed for secure message addressing and also needed interface interactions
to achieve basic searchability operations with directories. This common core set of capability of systems enables an agreed technical
profile for searching provider directories to obtain information required for secure message addressing.

FHIR<sup>&reg;&copy;</sup> connectathon events are key to the verification of the guide as being suitable for 
implementation. This implementation guide will be used as the basis for Australian connectathon events.

### How to read this guide

This guide is divided into several pages which are listed at the top of each page in the menu bar.

- [Home](index.html): This page provides the introduction and scope for this guide.
- [Conformance](conformance.html): These pages describe the set of rules to claim conformance to this guide including the expectations for *Must Support*  elements in AU PD profiles.
    - [General Requirements](general-requirements.html): This page defines requirements common to all actors and profiles used in this guide including how CapabilityStatements are used to claim conformance. This page defines the expectations for mandatory and *Must Support*  elements in AU PD Profiles.
    - [Declaring Conformance](declaring-conformance.html): This page describes how to declare conformance to AU PD.
- [Guidance](guidance.html): This page provides guidance on using the profiles defined in this guide.
    - [General Guidance](general-guidance.html): This page provides guidance on using the profiles defined in this guide.
    - [Sex and Gender](sex-and-gender.html): This page provides guidance on the representation of sex, gender, and related concepts.
    - [Relationship With Other IGs](relationship.html): This page provides guidance on the relationship between AU PD, AU Core, and other implementation guides.
    - [AU Variance Statement](variance.html): This page documents the variance from AU Base.
    - [Future of AU PD](future.html): This page outlines the approach to developing AU PD and yearly update cycle.
- [Security and Privacy](security.html): This page documents the AU PD general security and privacy requirements and recommendations.
- [FHIR Artefacts](artifacts.html): These pages provide detailed descriptions and formal definitions for all the FHIR artefacts defined in this guide.
  - [Profiles and Extensions](profiles-and-extensions.html): This set of pages describes the profiles and extensions that are defined in this guide to represent Australian local concepts using FHIR. Each profile page includes a narrative description, guidance, and formal definition. Although the guidance typically focuses on the profiled elements and seeks to provide a ‘how-to’ guide when representing concepts, it may also may focus on un-profiled elements to aid with implementation.
  - [Search Parameters](search-parameters.html): This set of pages lists the search parameters defined in this guide for use in AU operations.
  - [Terminology](terminology.html): This set of pages lists the value sets and code systems defined in this guide.
  - [Capability Statements](capability-statements.html): This page defines the expected FHIR capabilities of AU PD Servers and Clients.
  - [Actor Definitions](actors.html): This page describes the AU PD actors, AU PD Responder and AU PD Requester.
- [Federation](federation.html): This page outlines the arrangements and requirements for federated directories.
- [Examples](examples.html): This page lists all the examples used in this guide.
- [Support](support.html): These pages provide supporting material for implementation of AU PD.    
  - [Downloads](downloads.html): This page provides links to downloadable artefacts.
  - [License and Legal](license.html): This page outlines the license and legal requirements for material in AU PD.
- [Change Log](changes.html): This page documents the changes across versions of this guide.

### Collaboration
This guide is the product of collaborative work undertaken with participants from:

* Secure Messaging Technical Working Group
* [HL7 Australia FHIR Working Group](https://confluence.hl7.org/display/HAFWG/HL7+Australia+FHIR+Work+Group+Home)
* Australian FHIR Implementers Community 
* Australian Digital Health Agency
* Australian FHIR Implementers Community
* HL7 Australia Members 










