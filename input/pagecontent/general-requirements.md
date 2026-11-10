{::options toc_levels="1..4"/}

### Conforming to AU PD

There are two different ways to implement AU PD:
1. [Profile Only Support](#profile-only-support): system that represents digital health information using AU PD profiles without the expectation to implement any FHIR specified interactions.
1. [Profile Support + Interaction Support](#profile-support--interaction-support): system that represents digital health information using AU PD profiles (Profile Support) AND implements the [FHIR REST API](http://hl7.org/fhir/R4/http.html) interactions defined by an [AU PD Capability Statement](capability-statements.html) (Interaction Support).

The requirements of the FHIR standard and [FHIR Conformance Rules](http://hl7.org/fhir/conformance-rules.html) apply, and define the use of terms in this guide including the conformance verbs - **SHALL**, **SHALL NOT**, **SHOULD**, **SHOULD NOT**, **MAY**.

Implementers are advised to be familiar with the requirements of the FHIR standard when implementing AU PD, in particular:
- [FHIR Terminology requirements](http://hl7.org/fhir/R4/terminologies.html)
- [FHIR RESTful API](http://hl7.org/fhir/R4/http.html) 
- [FHIR Search](http://hl7.org/fhir/R4/search.html)
- [FHIR Resource formats](http://hl7.org/fhir/R4/resource.html)
- [FHIR Data Types](http://hl7.org/fhir/R4/datatypes.html)

### Conformance Artifacts

#### AU PD Profiles and Extensions
The [Profiles and Extensions](profiles-and-extensions.html) page lists the AU PD profiles and AU PD extensions defined for this implementation guide. An AU PD profile [StructureDefinition](http://hl7.org/fhir/R4/structuredefinition.html) defines the minimum elements, extensions, vocabularies and value sets that **SHALL** be present and constrains the way elements are used when conforming to the profile.

AU PD profile elements include mandatory and *Must Support* requirements. [Mandatory Elements](#mandatory-elements) are required and have a minimum cardinality > 0. [Must Support](#must-support-and-obligation) elements have defined conformance obligations in AU PD based on actor roles.

Systems may implement AU PD as: 
- [Profile Only Support](#profile-only-support) for a system declaring conformance to one or more AU PD profiles
- [Profile Support + Interaction Support](#profile-support--interaction-support) for a system declaring conformance to one or both of the [AU PD Capability Statements](capability-statements.html)

#### AU PD Capability Statements
The [AU PD Requester CapabilityStatement](CapabilityStatement-au-pd-requester.html) defines the conformance requirements and expectations of an [AU PD Requester](ActorDefinition-au-pd-actor-requester.html) actor responsible for initiating queries for information from an [AU PD Responder](ActorDefinition-au-pd-actor-responder.html). The complete list of FHIR profiles, REST API interactions, and search parameters that can be implemented by an AU PD Requester are defined in this capability statement. 

The [AU PD Responder CapabilityStatement](CapabilityStatement-au-pd-responder.html) defines the conformance requirements and expectations of an [AU PD Responder](ActorDefinition-au-pd-actor-responder.html) actor responsible for providing responses to queries submitted by an [AU PD Requester](ActorDefinition-au-pd-actor-requester.html). The complete list of FHIR profiles, REST API interactions, and search parameters that can be implemented by an AU PD Responder are defined in this capability statement. An AU PD Responder declares conformance to this list of capabilities based on the resource types and interactions it implements.

#### Profile Only Support
A system that represents digital health information using the content models of AU PD profiles without the expectation to implement any AU PD interactions can declare Profile Only Support to one or more AU PD profiles. 

A system that [declares conformance](declaring-conformance.html#profile-only-support) as Profile Only Support to an AU PD profile:
- **SHALL** correctly populate a resource with all mandatory elements specified in the profile’s definition.
- **SHALL** correctly populate a resource with all *Must Support* elements for which a value is known. 
- **SHALL** implement the requirements on [Missing Data](general-requirements.html#missing-data) when an element value is not known.

A system is not required to declare Profile Only Support to all AU PD profiles. For example, a system that implements the [AU PD PractitionerRole](StructureDefinition-au-pd-practitionerrole.html) profile is not required to also implement the [AU PD HealthcareService](StructureDefinition-au-pd-healthcareservice.html) profile. 

#### Profile Support + Interaction Support

Profile Support + Interaction Support refers to a system that represents digital health information using the content models of AU PD profiles AND implement the REST API interactions defined by the applicable AU PD capability statement.

An [AU PD Responder](ActorDefinition-au-pd-actor-responder.html) that declares Profile Support + Interaction Support conformance:
- **SHALL** [declare conformance](declaring-conformance.html#profile-support--interaction-support) to the [AU PD Responder Capability Statement](CapabilityStatement-au-pd-responder.html) in the server's CapabilityStatement.
- **SHALL** specify the implemented resource types, associated AU PD profiles, REST API interactions and search parameters in the server's CapabilityStatement.
- **SHALL** implement the [mandatory](#mandatory-elements) and/or [Must Support](#must-support-and-obligation) requirements for the elements defined as such in the implemented AU PD profiles.
- **SHALL** implement the required REST API interactions and search parameters as defined in the [AU PD Responder Capability Statement](CapabilityStatement-au-pd-responder.html#resourcesSummary1).

An [AU PD Requester](ActorDefinition-au-pd-actor-requester.html) that declares Profile Support + Interaction Support conformance:
- **SHALL** implement the [mandatory](#mandatory-elements) and/or [Must Support](#must-support-and-obligation) requirements for the elements defined as such in the implemented AU PD profiles.
- **SHALL** implement the required REST API interactions and search parameters as defined in the [AU PD Requester Capability Statement](CapabilityStatement-au-pd-requester.html#resourcesSummary1).

### Mandatory Elements
Mandatory elements are elements with minimum cardinality > 0. When an element is mandatory, the data is expected to always be present. Very rarely, it may not be, and in this circumstance the requirements in [Missing Data](#missing-data) **SHALL** be applied. An element can be both *Must Support* and mandatory, in which case the requirements for mandatory's Missing Data requirements apply as described in [Missing Must Support and Mandatory Data](general-requirements.html#missing-must-support-and-mandatory-data).

The convention in this guide is to mark all mandatory elements as *Must Support* unless they are nested under an optional element.

### Must Support and Obligation
Labelling an element *[Must Support](https://www.hl7.org/fhir/conformance-rules.html#mustSupport)* means that systems that produce or consume resources **SHALL** provide support for the element in some meaningful way. The FHIR standard does not define exactly what 'meaningful' support for an element means, but indicates that a profile **SHALL** make clear exactly what kind of support is required when an element is labelled as *Must Support*.

Because AU PD is a foundational standard, *Must Support* needs to be defined in a way that does not impede or prescribe what a system does with the data, so as not to impede each implementation’s ability to tighten and define expectations for use under their own business rules, regulations, policies, etc. There is also a challenge that comes from inheritance of *Must Support* flags into implementation guides that have strict definitions for *Must Support* (e.g., must be able to display this value to an end user). AU PD will only apply the *Must Support* flag on the elements that are necessary to support _minimum_ requirements and are expected to be flagged as *Must Support* across the majority of Australian FHIR implementation guides.

In AU PD, the meaning of *Must Support* is specified in terms of [Obligation Codes](https://hl7.org/fhir/extensions/CodeSystem-obligation.html) in [obligation extensions](https://hl7.org/fhir/extensions/StructureDefinition-obligation.html) on the element definition. These obligations can also be applied at more granular levels, such as individual data type choices, terminology bindings, identifiers, or sub-elements. The obligation codes used to define the minimum obligations of *Must Support* elements in this implementation guide are reiterated below.

*Must Support* on a profile element **SHALL** be interpreted as detailed in the [Interpreting Profile Elements Labelled Must Support](#interpreting-profile-elements-labelled-must-support) section, which includes additional narrative conformance requirements where agreed.

Actor | Code | Display | Definition | Notes
--- | --- | --- | --- | ---
[AU PD Responder](ActorDefinition-au-pd-actor-responder.html) | [SHALL:populate-if-known](https://hl7.org/fhir/extensions/CodeSystem-obligation.html#obligation-SHALL.58populate-if-known) | SHALL populate if known | Conformant applications producing resources SHALL correctly populate this element if they know a value for the element, but it is acceptable if the system is unable to ever know a value for the element. | This obligation does not impose a requirement to be able to know a value, unlike populate and able-to-populate which do. 'Knowing' an element means that a value for the element is available in memory, persistent store, or is otherwise available within the system claiming conformance.
[AU PD Responder](ActorDefinition-au-pd-actor-responder.html) | [SHOULD:populate](https://hl7.org/fhir/extensions/CodeSystem-obligation.html#obligation-SHOULD.58populate) | SHOULD populate | Conformant applications producing resources SHOULD include this element if a value is known and allowed to be shared.| This implementation obligation means that whenever the producer knows the correct value for an element, it should populate it. This is NOT the same as cardinality, as a 'populate' element can be omitted if no data exists or the data that exists is prohibited from being shared. Organizations might be required to formally document their rules for determining when they are 'able to share' as part of conformance testing processes. (e.g. regulatory, consent, etc.)
[AU PD Requester](ActorDefinition-au-pd-actor-requester.html) | [SHALL:no-error](https://hl7.org/fhir/extensions/CodeSystem-obligation.html#obligation-SHALL.58no-error) | SHALL not error if present | Conformant applications SHALL accept resources containing any valid value for the element without error. | Applications are still able to inform the user that a value cannot be processed correctly and may ignore the data, but applications aren't able to reject an instance merely because the element is present (which would be allowed for elements that do not have this obligation). A system MAY raise an error if the value provided is not valid or violates specific business rules. This obligation also applies to elements that only contain an extension in place of a value where (or equivalent), should either of these be allowed on the consumer obligations

*Must Support* elements are treated differently between [AU PD Responder](ActorDefinition-au-pd-actor-responder.html) and [AU PD Requester](ActorDefinition-au-pd-actor-requester.html) actors.

#### AU PD Responder
An AU PD Responder:
- **SHALL** correctly populate all *Must Support* elements for which a value is known.
- **SHALL** implement the requirements on [Suppressed Data](general-requirements.html#suppressed-data) when an element is NOT allowed to be shared.
- **SHALL** implement the requirements on [Missing Data](general-requirements.html#missing-data) when an element value is not known.

When a *Must Support* element requires additional or stronger obligation, this obligation is specified in the AU PD Responder [obligation extension](https://hl7.org/fhir/extensions/StructureDefinition-obligation.html) on the element definition.

#### AU PD Requester
An AU PD Requester:
- **SHALL** accept resources containing any valid value for *Must Support* elements without error.
- **SHALL** accept resources containing *Must Support* elements with [Missing Data](#missing-data) or [Suppressed Data](#suppressed-data) without error.

How the system processes the resource depends on local requirements that could align with obligation terms such as [reject invalid](https://hl7.org/fhir/extensions/CodeSystem-obligation.html#obligation-reject-invalid), [correctly handle](https://hl7.org/fhir/extensions/CodeSystem-obligation.html#obligation-handle), [persist](https://hl7.org/fhir/extensions/CodeSystem-obligation.html#obligation-persist), [display](https://hl7.org/fhir/extensions/CodeSystem-obligation.html#obligation-display), or [ignore](https://hl7.org/fhir/extensions/CodeSystem-obligation.html#obligation-ignore).

When a *Must Support* element requires additional or stronger obligation, this obligation is specified in the AU PD Requester [obligation extension](https://hl7.org/fhir/extensions/StructureDefinition-obligation.html) on the element definition.

#### Presentation of Must Support and Obligation in Profiles

All elements with *Must Support* in AU PD are accompanied by an explicit obligation that identifies the expectations for one or more actors. When rendered in an implementation guide, each profile is presented in different formal views under tabs labelled "Differential Table", "Key Elements Table", and "Snapshot Table". Elements flagged with *Must Support* and stated obligations in these views are represented by <span style="padding-left: 1px; padding-right: 1px; color: white; background-color: red" title="This element must be supported">S</span><span style="padding-left: 1px; padding-right: 1px; color: white; background-color: red" title="This element has obligations">O</span> as shown below. 

Implementers need to refer to the "Key Elements Table" (or the "Snapshot Table") to see the full set of elements that are mandatory or *Must Support* with obligations, terminology requirements and constraints (i.e. invariants).

##### Presentation of Must Support and Obligation in Raw Representations

When viewing the raw representation (e.g. XML or JSON) of a profile, elements with *Must Support* are denoted by `mustSupport` set to "true", and obligations are defined in the [Obligation Extension](https://hl7.org/fhir/extensions/StructureDefinition-obligation.html) as shown in the example below.

Example: AU PD PractitionerRole profile with *Must Support* and obligations on identifier.

~~~
{
    "resourceType" : "StructureDefinition",
    ...
    "url" : "http://hl7.org.au/fhir/pd/StructureDefinition/au-pd-practitionerrole",
    ...
           {
              "id" : "PractitionerRole.identifier",
              "extension" : [{
                "extension" : [{
                "url" : "code",
                "valueCode" : "SHALL:populate-if-known"
            },
            {
                "url" : "actor",
                "valueCanonical" : "http://hl7.org.au/fhir/pd/ActorDefinition/au-pd-actor-responder"
            }],
            "url" : "http://hl7.org/fhir/StructureDefinition/obligation"
            },
            {
                "extension" : [{
                "url" : "code",
                "valueCode" : "SHALL:no-error"
            },
            {
                "url" : "actor",
                "valueCanonical" : "http://hl7.org.au/fhir/pd/ActorDefinition/au-pd-actor-requester"
              }],
            "url" : "http://hl7.org/fhir/StructureDefinition/obligation"
            }],
            "path" : "PractitionerRole.identifier",
            "mustSupport" : true,
           },
    ...
}
~~~

#### Interpreting Profile Elements Labelled Must Support
The section is provided as additional support in understanding of *Must Support* and obligations on elements in AU PD. This section does not override the obligations defined for an actor - implementers also need to read the profile specific implementation guidance for any qualifying requirements placed on the obligations for a *Must Support* element.

Profiles defined in this implementation guide flag *Must Support* on elements (e.g. `PractitionerRole.identifier`) and sub-elements of a data type (e.g. `PractitionerRole.name.use`). 
The explanation on how to interpret *Must Support* for an element does not address rules defined in each profile - which may limit or extend what is allowed for each element.

##### Must Support - Primitive Elements
Primitive elements are single elements with a primitive value. If a primitive element is labelled as *Must Support*: 
- AU PD Responders **SHALL** correctly populate the element if a value is known. 
- AU PD Requesters **SHALL** accept resources without error if the element is present and contains any valid value.

For example, the AU PD Organisation Profile `Organization.name` element is a primitive string datatype. Therefore, when claiming conformance to this profile:
- AU PD Responders **SHALL** correctly populate a value in `Organization.name` if a value is known.
- AU PD Requesters **SHALL** accept the Organization resource without error if `Organization.name` is present and contains any valid value.

##### Must Support - Complex Elements
Complex elements are composed of primitive and other complex elements. Elements may have additional rules defined in the profile that also apply, e.g. invariants. Coded elements ([CodeableConcept](https://hl7.org/fhir/R4/datatypes.html#CodeableConcept), [Coding](https://hl7.org/fhir/R4/datatypes.html#Coding), and [code](https://hl7.org/fhir/R4/datatypes.html#code) datatypes) have additional rules defined by the [terminology binding](https://hl7.org/fhir/R4/terminologies.html#strength) in the profile.

If a complex element is labelled as *Must Support*:
- AU PD Responders **SHALL** correctly populate the element with at least one of the sub-element values if the value is known.
- AU PD Requesters **SHALL** accept resources without error if the element is present and contains any valid sub-element.

For example, the AU PD PractitionerRole Profile `PractitionerRole.telecom` element is labelled *Must Support* and has no *Must Support* sub-elements. When claiming conformance to this profile:
- AU PD Responders **SHALL** correctly populate a value in any valid `PractitionerRole.telecom` sub-element if a value is known e.g. `PractitionerRole.telecom.value`.
- AU PD Requesters **SHALL** accept the PractitionerRole resource without error if `PractitionerRole.telecom` is present and contains any valid sub-elements.

If a sub-element is labelled as *Must Support*: 
- AU PD Responders **SHALL** correctly populate the element with all *Must Support* sub-elements for which a value is known. 
- AU PD Requesters **SHALL** accept resources without error if *Must Support* sub-elements are present and contains any valid value.

For example, in the AU PD Practitioner Profile, the `Practitioner.name` element is labelled *Must Support* and has *Must Support* sub-elements `Practitioner.name.family` and `Practitioner.name.given`. When claiming conformance to this profile:
- AU PD Responders **SHALL** correctly populate a value in `Practitioner.name.family` and `Practitioner.name.given` if the value for those sub-elements is known.
- AU PD Requesters **SHALL** accept a Practitioner resource without error if `Practitioner.name` is present and contains valid values in `Practitioner.name.family` and `Practitioner.name.given` sub-elements.

##### Must Support - Resource References
Some elements labelled as *Must Support* reference multiple resource types or profiles such as `PractitionerRole.practitioner`. In such cases: 
- AU PD Responders **SHALL** correctly populate the element with at least one referenced resource or allowed profile if the value is known. 
- AU PD Requesters **SHALL** accept resources without error if the element is present and contains any valid referenced resource or profile.

Refer to each profile's definition for the full list of allowed referenced resource types or profiles.


##### Must Support - Choice of Data Types
Some elements labelled as *Must Support* allow different data types. In such cases:
- AU PD Responders **SHALL** correctly populate the element with at least one data type allowed by the element definition if the value is known.
- AU PD Requesters **SHALL** accept resources without error if the element is present and contains any valid data type allowed by the element definition.

Refer to each profile's definition for the full list of elements that allow multiple data types.

##### Must Support - Identifiers
A profile may define support for one, none, or many identifier types. Supported identifier types are included in a profile by slicing the identifier element and placing *Must Support* on each identifier slice. 

Where no identifier slice is included in a profile, there is no expectation that a specific identifier type is supported and the section on [Must Support - Complex Elements](general-requirements.html#must-support---complex-elements) applies.

Where one or more identifier types are supported:
- AU PD Responders **SHALL** correctly populate the identifier element with identifiers from at least one supported identifier type where an identifier of that type is known, or any other identifier type when an identifier is known but it is not one of the supported identifier types.
- AU PD Requesters **SHALL** accept resources without error if the identifier element is present and contains any identifier type allowed by the element definition.

The table below provides a list of AU PD profile elements with one or more supported identifier types.

AU PD Profile |Must Support Element|Supported Identifiers
---|---|---
[AU PD Organisation](StructureDefinition-au-pd-organisation.html)|Organization.identifier|HPI-O
[AU PD Practitioner](StructureDefinition-au-pd-practitioner.html)|Practitioner.identifier|HPI-I
[AU PD PractitionerRole](StructureDefinition-au-pd-practitionerrole.html)|PractitionerRole.identifier|Medicare Provider Number
{:.grid}

For example, the profile [AU PD Organisation](StructureDefinition-au-pd-organisation.html) defines support for the Healthcare Provider Identifier - Organisation (HPI-O) as a slice of `Organization.identifier` labelled with *Must Support*. When claiming conformance to the AU PD Organisation Profile:
- AU PD Responders **SHALL** correctly populate `Organization.identifier` with an HPI-O if known, or any other identifier type when an HPI-O is not known but some other identifier is known.
- AU PD Requesters **SHALL** accept the Organization resource if `Organization.identifier` is present and contains any valid value.

Systems **MAY** support populating and accepting other identifiers, but this is not a requirement of AU PD.

##### Must Support - Choice of Profile Elements

A profile may label two or more elements as *Must Support* that are used to represent the same concept. In such cases:
- AU PD Responders **SHALL** correctly populate at least one element choice if the value is known.
- AU PD Requesters **SHALL** accept resources without error if any element allowed by the profile is present and contains any valid value.


##### Must Support - Multiple Terminologies
A coded element can have support defined for one or many value sets. Coded elements that define support for more than one value set include them in a profile by slicing the [Coding](http://hl7.org/fhir/R4/datatypes.html#Coding) part of the element and placing *Must Support* on each value set slice. These value set slices are not intended to prevent systems from supplying only a text value.

Most coded *Must Support* elements in AU PD define support for one value set, which is bound to the element and no value set slice is present. For these elements, the section on [Must Support - Complex Elements](general-requirements.html#must-support---complex-elements) and the terminology binding applies.

Where multiple value sets are supported:
- AU PD Responders **SHALL** correctly populate the element with concepts from all supported value sets where the applicable concept is known.
- AU PD Requesters **SHALL** accept resources without error if the element is present and contains any valid value. 

Refer to each profile's definition for the full list of elements that support multiple value sets.

Systems **MAY** populate and accept concepts from other value sets but this is not a requirement of AU PD.

### Missing Data

There are situations when information for a particular data element is missing and the source system does not know the reason for the absence of data. 

#### Missing Must Support and Optional Data

If the source system does not know the value for an optional element (minimum cardinality = 0), including elements labelled *Must Support*, the data element **SHALL** be omitted from the resource.  

#### Missing Must Support and Mandatory Data

If the data element is a mandatory element (minimum cardinality is > 0), the element **SHALL** be present *even if* the source system does not know the value or the reason the value is absent.

1.  For *non-coded* data elements where the applicable AU PD profile does not mandate a sub-element
    - use the [DataAbsentReason extension](http://hl7.org/fhir/R4/extension-data-absent-reason.html) 
    - use the code "unknown": _The value is expected to exist but is not known_
  
    Example: Organization resource where the organization's name is not available.
    ~~~
    {
      "resourceType" : "Organization",
           ...
           "_name" : {
                "extension" : [
                  {
                    "url" : "http://hl7.org/fhir/StructureDefinition/data-absent-reason",
                    "valueCode" : "unknown"
                  }
                ]
              },
          ...
         }
    ~~~
      
2. For *non-coded* data elements where the applicable AU PD profile mandates a sub-element, e.g. in AU PD Practitioner, the sub-element `Practitioner.name.family` is mandatory, the resource must contain the sub-element otherwise the resource will not be conformant.

3. For *coded* data elements:
    - *required* binding strength:
      - use the appropriate "unknown" concept code from the value set if available.
      - if the value set does not have the appropriate "unknown" concept code, you must use a concept from the value set anyway. Otherwise, the instance will not be conformant.
      Refer to each AU PD profile for the applicable status elements with required binding that have no appropriate "unknown" concept code.
    - *example*, *preferred*, or *extensible* binding strengths:
      - when the system has text but no coded value, only the text sub-element is populated.
      - when there is neither text or coded value:
        - use the appropriate "unknown" concept code from the value set if available.
        - when the value set does not have an appropriate "unknown" concept code, use "unknown" from the [DataAbsentReason Code System](http://terminology.hl7.org/CodeSystem/data-absent-reason).

        Example: PractitionerRole resource where the specialty is unknown.
        ~~~
        ...
        "specialty" : [
          {
            "coding" : [
              {
                "system" : "http://terminology.hl7.org/CodeSystem/data-absent-reason",
                "code" : "unknown",
                "display" : "Unknown"
              }
            ]
          }
        ]
        ...
        ~~~


### Suppressed Data
In some circumstances, specific pieces of data may be hidden due to security or privacy reasons. 

When an element definition is optional (minimum cardinality = 0), including elements labelled *Must Support*:
- The element **SHALL NOT** be populated in the resource when a value is suppressed.  

When an element definition is mandatory (minimum cardinality > 0), 
- The element **SHALL** be correctly populated but it may exceed the data requester's access rights to know that the data is suppressed:
  - where a requester does not have access rights to know that data is suppressed use the code "unknown" from the [DataAbsentReason Code System](http://terminology.hl7.org/CodeSystem/data-absent-reason) following the section on [Missing Data](#missing-data).
    
    Example: Organization resource where the identifier is suppressed.
    ~~~
    "resourceType" : "Organization",
    ...
    "identifier" : [
      {
        "extension" : [
          {
            "url" : "http://hl7.org/fhir/StructureDefinition/data-absent-reason",
            "valueCode" : "unknown"
          }
        ]
      }
    ],
    ...
    ~~~

  - where a requester has the access rights to know that data is suppressed use the code "masked" from the [DataAbsentReason Code System](http://terminology.hl7.org/CodeSystem/data-absent-reason) following the section on [Missing Data](#missing-data).
  
    Example: PractitionerRole resource where the specialty is suppressed.
    ~~~
    {
      "resourceType" : "PractitionerRole",
      ...
      "specialty" : [{
        "coding" : [
          {
            "system" : "http://hl7.org/fhir/StructureDefinition/data-absent-reason",
            "code" : "masked"
          }
        ]
      }],
      ...
    }
    ~~~
