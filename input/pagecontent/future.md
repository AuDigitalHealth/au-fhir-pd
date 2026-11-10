### AU PD and AU Core Alignment
AU PD profiles are derived from AU Core profiles. When AU Core publishes a new release, AU PD will be updated to align with the latest AU Core version. Changes in AU Core that affect the resources profiled in AU PD (Practitioner, PractitionerRole, Organization, Location, HealthcareService, Endpoint) will be reflected in a corresponding AU PD update.

### Future Candidate Requirements Under Consideration
The following items are under consideration for future versions of AU PD.

* **Additional Endpoint Types**: The current AU PD scope covers secure messaging endpoints. Future versions may expand endpoint support to cover additional communication channels such as FHIR RESTful endpoints and other digital health messaging services.
* **Additional Search Parameters**: Future versions of AU PD will include a more complete set of FHIR SearchParameter definitions to formally describe the search capabilities declared in the AU PD CapabilityStatements, including chained search parameters.
* **Record or Update Data**: Systems may use operations that create and update provider directory resources. More guidance and minimum requirements on writing and updating directory data in the context of AU PD implementations will be developed in future versions.
* **Synchronisation Guidance**: Additional guidance on the synchronisation pattern for keeping local directories up to date with a source-of-truth provider directory service will be developed in future versions.
* **Validation Requirements**: Future versions of AU PD may introduce additional invariants and terminology bindings to support consistent validation of provider directory entries across implementations.