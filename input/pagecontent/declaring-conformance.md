### Declaring Conformance

<a id="profile-only-support"></a>
<a id="profile-support--interaction-support"></a>

Systems may declare conformance to AU PD as a:
- [Provider Directory Supplier](general-guidance.html#provider-directory-supplier) by declaring conformance to the [AU PD Responder Capability Statement](CapabilityStatement-au-pd-responder.html)
- [Provider Directory Consumer](general-guidance.html#provider-directory-consumer) by declaring conformance to the [AU PD Requester Capability Statement](CapabilityStatement-au-pd-requester.html)

#### Provider Directory Supplier

Servers that implement AU PD as a Provider Directory Supplier declare conformance by hosting a [CapabilityStatement](http://hl7.org/fhir/capabilitystatement.html) resource at [server-base-url]/metadata that is available to both authenticated and unauthenticated clients. The server's [CapabilityStatement.instantiates](http://hl7.org/fhir/capabilitystatement-definitions.html#CapabilityStatement.instantiates) element references the [AU PD Responder Capability Statement](CapabilityStatement-au-pd-responder.html), as shown in the following fragment:

    ```
    {
      "resourceType": "CapabilityStatement",
      ...
      "instantiates": [
        "http://hl7.org.au/fhir/pd/CapabilityStatement/au-pd-responder"
      ],
      ...
      "rest": [
        {
          "mode": "server",
          ...
          "resource": [
            {
              "type": "PractitionerRole",
              "profile": "http://hl7.org.au/fhir/pd/StructureDefinition/au-pd-practitionerrole",
              ...
            }
          ]
        }
      ]
    }
    ```

#### Provider Directory Consumer

Clients that implement AU PD as a Provider Directory Consumer declare conformance by hosting a [CapabilityStatement](http://hl7.org/fhir/capabilitystatement.html) resource at [server-base-url]/metadata that is available to both authenticated and unauthenticated clients. The client's [CapabilityStatement.instantiates](http://hl7.org/fhir/capabilitystatement-definitions.html#CapabilityStatement.instantiates) element references the [AU PD Requester Capability Statement](CapabilityStatement-au-pd-requester.html), as shown in the following fragment:

    ```
    {
      "resourceType": "CapabilityStatement",
      ...
      "instantiates": [
        "http://hl7.org.au/fhir/pd/CapabilityStatement/au-pd-requester"
      ],
      ...
      "rest": [
        {
          "mode": "client",
          ...
        }
      ]
    }
    ```