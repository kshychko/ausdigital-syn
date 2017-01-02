# JSON Syntax for UBL elements

The normative form for UBL documents is XML and the normative definition is the UBL XSD Schema lubrary.  Therefore the JSON syntax implementation for UBL occasionally refers to the XML form when defining the JSON rules.

## JSON Schema Rules

* Each UBL document MUST be described by a JSON Schema document that complies with the [JSON Schema Specification](http://json-schema.org/documentation.html) and contains the JSON element "$schema": "http://json-schema.org/draft-04/schema#".  
* The JSON Schema MUST have an "id" element that contains the URI of the normative JSON Schema of the form https://ausdigital.org/{domain}-semantics/spec/vx.y.z/document.json - for example https://github.com/ausdigital/billing-semantics/spec/v1.0.0/Invoice.json
* The JSON schema MUST be a standalone single root schema that does not import external fragments but MAY make use of local schema references ($ref) for re-use of common structures.  It is recognised that several different document schema will define the same common structures but this SHOULD be managed at the semantic model level and not at the runtime schema level.
* JSON type definitions MUST use exactly the same name as the corresponding UBL XML element name but without any namespace prefix (ie use the UNQUALIFIED UBL element name).
* JSON schema type definitions MUST include a "description" property and it's value SHOULD equal the corresponding UBL schema <ccts:Definition> property.
* JSON schema type definitions SHOULD NOT use "enum" types for validation of code list values.  This is to allow codes lists to be managed and versioned searpately from their use in business document schema.  Please refer to the [code lists](https://github.com/ausdigital/code-lists) specification for the treatment of code lists.

## CCTS Property Mapping

All UBL types inherit from the UN/CEFACT Core Component Technical Specification (CCTS) set of 10 core data types. Each CCTS data type has a number of optional properties that are implemented as UBL XML element attributes - for details please refer to the [UBL CCTS Schema Module](https://docs.oasis-open.org/ubl/cs-UBL-2.0/xsd/common/CCTS_CCT_SchemaModule-2.0.xsd).  There are two concerns with mapping CCTS properties to JSON.
* JSON has no equivalent of XML element attributes
* All the CCTS attributes are optional with often overlapping definitions and little implementation guidance for allowed values.  This leads to confusion and interoperability problems. For example, 8 of the 9 attributes of the CCTS "Code" type all serve to identify different attributes of the code list scheme (listID, listAgencyID, listAgencyName, listName, listVersionID, listURI, listSchemeURI).  A single unique identifier of the code list scheme would be sufficient.

In mapping UBL to JSON there is a balance to be struck between a simple mapping algorithm (leading to relatively more complext JSON) and a more complex set of mapping rules (that could deliver simpler JSON).  This specification prioritises simple JSON even at the cost of more complex mapping rules when transforming to/from UBL XML.

|CCTS Type | UBL XML Implementation | JSON Implementation|Notes |
|--------------|------------|--------------------|------|
|AmountType|```<cbc:Amount currencyID="AUD" currencyCodeListVersionID="1996">10.00</cbc:Amount>```  |"Amount":10.00| No need to specify currecy for every data element.  Instead use the UBL document level currency indicator "documentCurrencyCode":"AUD" |
|BinaryObjectType  |```<cbc:EmbeddedDocumentBinaryObject format="?" characterSetCode="?" mimeCode="application/pdf" encodingCode="?" uri="?" filename="invoice1234.pdf"> UjBsR09EbGhjZ0dTQUxNQU </cbc:EmbeddedDocumentBinaryObject>```|  | |
|  |  |  |  |
|  |  |  |  |
|  |  |  |  |
|  |  |  |  |
|  |  |  |  |
|  |  |  |  |
|  |  |  |  |
|  |  |  |  |






