
The `AU PD Practitioner` profile constrains the base `Practitioner` resource for use in Australian national provider-directory and Provider Directory (PD) exchanges. It provides the national baseline for person-level practitioner data (identifiers, names, qualifications, limited contact) suitable for PD publishing, search and responder/requester interactions; it does not carry role-specific information such as organisational affiliation, locations or role‑level contact — use `PractitionerRole` for that purpose.

This profile is derived from `AU Core Practitioner`. It inherits sex/gender guidance and terminology bindings from those profiles. Where AU Core provide normative guidance (for example recorded sex or gender, pronouns and name handling), AU PD follows that behaviour unless explicitly documented below.

### Usage Scenarios

The following are supported usage scenarios for this profile:

- Publish or update a practitioner's national identifiers, name, professional qualifications and registration details for inclusion in a national provider directory.
- Search a provider directory for a practitioner by identifier (for example HPI-I, Ahpra registration number, PBS prescriber number), name or registered profession.
- Retrieve a practitioner's demographic and qualification details when referenced by another resource (for example `PractitionerRole`).
- Determine whether a practitioner record is currently active, suppressed or deactivated before displaying it to a directory consumer.
- Resolve limited person-level contact details (where explicitly consented) in support of directory listing use cases.

### Profile Specific Implementation Guidance

#### Qualification & professional registration
- Represent professional registration in `Practitioner.qualification`.
- Populate `qualification.identifier` with the registration number (source system in `identifier.system` or `identifier.assigner`).
- Populate `qualification.code.text` with a human-readable profession label — this is REQUIRED when the profession is known. Add `qualification.code.coding` only when you have an authoritative coding system mapping (Ahpra or recognised peak body); always provide `code.text` when coding is present.

#### Extensions, sensitive data and privacy
- Personal contact details (personal phone/email): treat as sensitive. Only publish and display where explicit practitioner consent is recorded; publishing systems MUST document consent provenance and restrict public exposure accordingly.
- Sex/gender: follow AU Base / AU Core guidance. For public PD, prefer `recordedSexOrGender` (RSG) when search/filtering is required; avoid exposing `gender` or `genderIdentity` as public search parameters unless explicitly authorised.
