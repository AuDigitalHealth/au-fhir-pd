
The `AU PD PractitionerRole` profile constrains the base `PractitionerRole` resource for use in national provider-directory (PD) exchanges. It provides the national PD baseline for role-specific practitioner data (role, organisation, service, location, contact and endpoint details) used by directory publishing and discovery services; use `Practitioner` for person-level identifiers and qualifications.

This profile complements the `AU PD Practitioner` person-level profile: prefer `PractitionerRole` to convey role-specific location, service, organisational affiliation, role-level telecom and endpoints. `Practitioner` remains the authoritative record for person-level demographics and identifiers.

### Usage Scenarios

The following are supported usage scenarios for this profile:

- Publish or update a practitioner's role, service, location and contact details at a specific organisation for inclusion in a national provider directory.
- Search a provider directory for practitioners offering a given service or specialty at a location or organisation.
- Retrieve role-specific contact points, locations and digital endpoints (e.g., online booking, secure messaging) associated with a practitioner's role, without exposing person-level identifiers.
- Resolve a `PractitionerRole` to its associated `Practitioner` (and vice versa) to combine person-level and role-level information for display or referral purposes.
- Determine whether a practitioner is currently active in a given role at an organisation, including handling of deactivated or suppressed roles.

#### Role details (service, specialty, location, contact)
- Use `PractitionerRole.code` / `specialty` to describe services provided and specialty. Bindings should follow AU Base/AU Core recommended value sets when available; include `code.text` for human-readable service labels.
- Provide `PractitionerRole.location` references when the role is associated with specific sites; include `telecom` entries on the role for role-specific contact points (e.g., clinic phone, role-specific fax).
- Use `endpoint` references to link to `Endpoint` resources that describe digital channels (APIs, messaging endpoints) or online booking/contact channels relevant to this role.

#### Endpoints and communication channels
- Include `PractitionerRole.endpoint` for machine-discoverable channels. When an endpoint represents a public service (e.g., online booking), populate `Endpoint.address` and `Endpoint.payloadType` appropriately.
- For directories, include guidance on expected endpoint types (e.g., `rest`, `hl7-messaging`, `sms`) and the intended use (appointments, secure messaging, API access).

