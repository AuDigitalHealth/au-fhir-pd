This guide is built on top of the FHIR standard and, where available, [AU Core](https://hl7.org.au/fhir/core/2.0.0/index.html), and [AU Base](https://hl7.org.au/fhir/6.0.0/index.html).

The context of AU PD within the set of HL7 AU standards is shown in the figure below:
- the **FHIR standard** is the foundation, which creates a common platform or foundation on which a variety of different solutions are implemented. References to the FHIR standard on this page include the HL7 International Core FHIR Specification, HL7 International Core Extensions FHIR Implementation Guide (Extensions Pack) and HL7 International HL7 Terminology (THO). 
- **AU Base** defines local concepts for use in an Australian context introducing relevant identifiers, terminology, extensions. 
- **AU Core** defines a set of conformance requirements that enforce a set of ‘minimum requirements’ on the local concepts from AU Base, specifying rules for the elements, extensions, vocabularies, and value sets, and the RESTful API interactions.
- **Use case IGs** (AU eRequesting) build on AU Core to address specific use cases, defining a set of conformance requirements on top of AU Core, using additional building blocks from AU Base as needed.

This layering of IGs balances relative adoption and implementation maturity of FHIR and requirements of the use cases involved.

  <div> 
    <img src="architecture.png" alt="Context of AU Provider Directory within the set of HL7 AU standards" style="width:65%"/>
  </div>
*Figure 1: Context of AU PD within the set of HL7 AU standards*
<br/>

### Relationship to AUCore and Other IGs
AU PD is a technical specification that addresses constraints and obligations on data representation for exchange. AU PD enables system data to be mapped to an agreed FHIR format, produced, and consumed by systems in Australia.

AU PD is based on, aligns to, and leverages international standards and other national standards. Corresponding profiles included in relevant FHIR implementation guides were reviewed and considered during the development process to ensure alignment and to facilitate adoption of this standard. These implementation guides include:
- [HL7 Cross Paradigm Implementation Guide: Gender Harmony - Sex and Gender Representation, Edition 1](https://hl7.org/xprod/ig/uv/gender-harmony/informative1/)
<!-- - [International Patient Access 1.1.0](https://hl7.org/fhir/uv/ipa/STU1.1/)
- [International Patient Summary 2.0.1](https://hl7.org/fhir/uv/ips/2.0.1/en/) -->
- [AU Core 2.0.0](https://hl7.org.au/fhir/core/2.0.0/index.html)

The relationship of AU PD to AUCore and other implementation guides is shown in the figure below.

TBD

 <!-- <div> 
    <img src="context-colour.png" alt="Relationship to AUCDI and Other IGs" style="width:75%"/>
  </div>
*Figure 2: Relationship to AUCDI and Other IGs*
<br/> -->

Implementation Guide |Relationship
---|---
[AU Base](https://hl7.org.au/fhir/6.0.0/index.html)|This IG defines Australian realm concepts including terminology, identifiers, and extensions. AU PD uses AU Base, where available, as the basis for profiles that define the FHIR resources to be supported, and the elements, extensions, vocabularies, and value sets that SHALL be present are identified, and how they are used is defined. Where available and applicable, AU Base concepts are profiled in AU Core and inherited by AU PD.
[HL7 Cross Paradigm Implementation Guide: Gender Harmony - Sex and Gender Representation](https://hl7.org/xprod/ig/uv/gender-harmony/informative1/)|This IG provides definitive guidance on how to exchange clinical sex and gender affirming information using HL7 models. Sex and gender concepts from this IG have been reviewed for the potential for adoption in Australia. Where adopted, these concepts are included by reference in AU Base, profiled in AU Core, and inherited by AU PD.
[AU Core](https://hl7.org.au/fhir/core/2.0.0/index.html)|This IG defines a set of conformance requirements that enforce a set of 'minimum requirements' on the local concepts from AU Base, specifying the elements, extensions, vocabularies, and value sets that SHALL be present and how they SHALL be used. AU Core also defines a data access API, specifying the conformance requirements for RESTful interactions. AU PD is compliant with the profile only requirements of AU Core, e.g. AU Core conformant data can be used to generate an AU PD document and AU PD conformant data can be accessed by an AU Core conformant requester.
{:.grid}


