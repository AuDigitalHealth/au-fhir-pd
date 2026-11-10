This version of the AU Provider Directory FHIR IG complies with [Version 1.0 of the HL7 Australia FHIR Work Group AU Variance Requirements](https://hl7.org.au/fhir/HL7%20AU%20Variance%20Requirements%20v1.0.pdf).

HL7 Australia published FHIR implementation guides are required to follow specific publishing requirements:
- **SHOULD** use AU Core profiles
- **SHOULD** use AU Base profiles and extensions
- **SHALL** include an AU Variance Statement page

### Variance from AU Core
This implementation guide has no variance (i.e. fully compliant) from AU Core FHIR Implementation Guide version 3.0.0-ballot1 ([current](https://build.fhir.org/ig/hl7au/au-fhir-core/)).

All AU PD profiles are derived from AU Core profiles:
- [AU PD Practitioner](StructureDefinition-au-pd-practitioner.html) derives from [AU Core Practitioner](https://build.fhir.org/ig/hl7au/au-fhir-core/StructureDefinition-au-core-practitioner.html)
- [AU PD Practitioner Role](StructureDefinition-au-pd-practitionerrole.html) derives from [AU Core PractitionerRole](https://build.fhir.org/ig/hl7au/au-fhir-core/StructureDefinition-au-core-practitionerrole.html)
- [AU PD Organisation](StructureDefinition-au-pd-organisation.html) derives from [AU Core Organization](https://build.fhir.org/ig/hl7au/au-fhir-core/StructureDefinition-au-core-organization.html)
- [AU PD Location](StructureDefinition-au-pd-location.html) derives from [AU Core Location](https://build.fhir.org/ig/hl7au/au-fhir-core/StructureDefinition-au-core-location.html)
- [AU PD Healthcare Service](StructureDefinition-au-pd-healthcareservice.html) derives from [AU Core HealthcareService](https://build.fhir.org/ig/hl7au/au-fhir-core/StructureDefinition-au-core-healthcareservice.html)
- [AU PD Secure Messaging Endpoint](StructureDefinition-au-pd-sm-endpoint.html) derives from [AU Core Endpoint](https://build.fhir.org/ig/hl7au/au-fhir-core/StructureDefinition-au-core-endpoint.html)

#### Additionally Defined Profiles

This implementation guide defines the following identifier profiles that are not profiled in AU Core:
- [AU PD Secure Messaging Delivery Target Identifier](StructureDefinition-au-pd-smdtargetidentifier.html) profiles FHIR data type [Identifier](https://hl7.org/fhir/R4/datatypes.html#Identifier)
- [AU PD Vendor Directory Identifier](StructureDefinition-au-pd-vdi.html) profiles FHIR data type [Identifier](https://hl7.org/fhir/R4/datatypes.html#Identifier)