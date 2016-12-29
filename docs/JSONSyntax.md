# JSON Syntax for UBL elements

The normative form for UBL documents is XML and the normative definition is the UBL XSD Schema lubrary.  Therefore the JSON syntax implementation for UBL occasionally refers to the XML form when defining the JSON rules.

## JSON Schema

* Each UBL document MUST be described by a JSON Schema document that complies with the [JSON Schema Specification](http://json-schema.org/documentation.html) and contains the JSON element "$schema": "http://json-schema.org/draft-04/schema#".  
* The JSON Schema MUST have an "id" element that contains the URI of the normative JSON Schema of the form https://ausdigital.org/{domain}-semantics/spec/vx.y.z/document.json - for example https://github.com/ausdigital/billing-semantics/spec/v1.0.0/Invoice.json
* The JSON schema MUST be a standalone single root schema that does not import external fragments but MAY make use of local schema references ($ref) for re-use of common structures.  It is recognised that several different document schema will define the same common structures but this SHOULD be managed at the semantic model level and not at the runtime schema level.


## JSON Schema Type Definitions

* JSON type definitions MUST use exactly the same name as the corresponding UBL XML element name but without any namespace prefix (ie use the UNQUALIFIED UBL element name).
* JSON schema type definitions MUST include a "description" property and it's value SHOULD equal the corresponding UBL schema <ccts:Definition> property.
* JSON schema type definitions MUST include a "description" property and it's value SHOULD equal the corresponding UBL schema <ccts:DictionaryEntryName> property.
* JSON schema type definitions SHOULD NOT use "enum" types for validation of code list values.  Please refer to the [common semantics](https://github.com/ausdigital/common-semantics) specification for the treatment of code lists.

## CCTS Property Mapping

All UBL types inherit from the UN/CEFACT Core Component Technical Specification (CCTS) set of 10 core data types. Each CCTS data type has a number of optional properties that are implemented as UBL XML element attributes.  There are two concerns with mapping CCTS properties to JSON.
* JSON has no equivalent of XML element attributes
* All the CCTS attributes are optional with often overlapping definitions and little implementation guidance for allowed values.  This leads to confusion and interoperability problems. For example, 8 of the 9 attributes of the CCTS "Code" type all serve to identify different attributes of the code list scheme (listID, listAgencyID, listAgencyName, listName, listVersionID, listURI, listSchemeURI).  A single unique identifier of the code lust scheme would be sufficient.

Whilst a simple approach to mapping would be to create a JSON object for every UBL element with properties for each CCTS type attribute, that would lead to an un-necessarily complex JSON implementation that would just inherit the same porblems and thereby defeat the purpose of a simple JSON model.  Therefore the preferred approach is to define more explicity JSON mapping rulles for each CCTS type.

To-do : list of types, attributes, and JSON mappings.






