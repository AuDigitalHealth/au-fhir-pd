This page describes how sex and gender related concepts are handled for the AU PD profiles.

AU PD profiles are derived from AU Core profiles, which in turn inherit sex and gender elements localised in AU Base. AU Base follows the logical data element models provided by the HL7 Gender Harmony (Sex and Gender Representation) guidance.

Of the AU PD profiles, only [AU PD Practitioner](StructureDefinition-au-pd-practitioner.html) involves a person for whom sex and gender concepts are applicable. `AU PD Practitioner` is derived from [AU Core Practitioner](https://build.fhir.org/ig/hl7au/au-fhir-core/StructureDefinition-au-core-practitioner.html) and does not introduce additional constraints on sex and gender elements. Sex and gender handling for AU PD therefore follows AU Core Practitioner and the AU Base Sex and Gender guidance.

The short reference below highlights the concepts relevant to AU PD implementers and where to find the authoritative guidance.

### Key concepts (AU PD scope)

- Administrative Gender (`Practitioner.gender`): Not *Must Support* in AU PD Practitioner. See the AU Base [Administrative Gender](https://build.fhir.org/ig/hl7au/au-fhir-base/sexgender.html#administrative-gender-patientgender) guidance and the inherited constraints from `AU Core Practitioner`.
- Name to Use (NtU, `name.use`): Not *Must Support* in AU PD Practitioner. Follow AU Base guidance for Name to Use.
- Pronouns (`individual-pronouns` extension): Not *Must Support* in AU PD Practitioner. AU Core/ AU Base define usage and value sets where applicable.
- Gender Identity (`individual-genderIdentity` extension): Not *Must Support* in AU PD Practitioner. Follow AU Base and AU Core guidance for recommended extensions and value sets.
- Recorded Sex or Gender: Not *Must Support* in AU PD Practitioner. Refer to AU Base guidance for recommended handling.
- Sex Assigned at Birth: Not *Must Support* in AU PD Practitioner. AU Core contains a profiled implementation (AU Core Sex Assigned At Birth) which AU PD inherits if needed.
- Sex Parameter for Clinical Use (SPCU): Not applicable for AU PD Practitioner in most PD use-cases; refer to AU Base for clinical guidance.

### Guidance for implementers

- Follow AU Base Sex and Gender page for authoritative definitions and the HL7 Gender Harmony IG for semantics.
- For AU PD exchanges, treat sex/gender elements as optional unless your business requirements or local policy mandate support. AU PD does not add any PD-specific *Must Support* requirements for these elements.
- When in doubt, align with the constraints and *Must Support* decisions published in AU Core profiles (for example, `AU Core Patient` and `AU Core Practitioner`) since AU PD inherits their behaviour for person-level elements.

If you would like a compact AU PD-only decision table or PD-specific examples for `Practitioner` instances, I can add those next.
