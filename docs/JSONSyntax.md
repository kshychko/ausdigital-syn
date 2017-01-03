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
|AmountType|```<cbc:Amount currencyID="AUD" currencyCodeListVersionID="1996">10.00</cbc:Amount>```  |```"Amount":10.00``` | JSON representation will not specify currecy for every data element.  Instead it will use the UBL document level currency indicator "documentCurrencyCode":"AUD" |
|BinaryObjectType  |```
<cbc:EmbeddedDocumentBinaryObject format="pdf" characterSetCode="utf-8" mimeCode="application/pdf" encodingCode="base64" uri="https://mydomain.com/invoice1234.pdf" filename="invoice1234.pdf">
UjBsR09EbGhjZ0dTQUxNQU
</cbc:EmbeddedDocumentBinaryObject>
```| ```"ExternalReference":{"URI":uri="https://mydomain.com/invoice1234.pdf", "DocumentHash":"H5DGjk67g3SDllk"}``` |UBL supports either embedded or referenced binary attachments.  The JSON implementaiton will always use the UBL "ExternalReference" Model |
|CodeType  |```<cac:Country> <cbc:IdentificationCode listAgencyID="5" listAgencyName="International Organization for Standardization" listName="Country Identification Code" listVersionID="SecondEdition2006VI-12" name="CountryIdentificationCode" languageID="en" listURI="http://docs.oasis-open.org/ubl/os-UBL-2.1/cl/gc/default/CountryIdentificationCode-2.1.gc" listSchemeURI="urn:un:unece:uncefact:identifierlist:standard:5:ISO316612A"> AU </cbc:IdentificationCode> </cac:Country>``` | ```"Country":"AU"``` |The reference codelist scheme for each coded element in a JSON UBL is defined as part of the implementation profile and is referenced through the CustomizationID as described in the [code-list specification](http://code-lists.readthedocs.io/en/latest/).|
|DateTimeType  | ```<cbc:IssueDate format="iso-8601">2016-07-01</cbc:IssueDate>``` | ```"issueDate":"2016-07-01"``` | JSON representation will always use ISO-8601 standard datetime format. |
|IdentifierType  |```      <cac:PartyIdentification> <cbc:ID schemeID="urn:oasis:names:tc:ebcore:partyid-type:iso6523:0151" schemeName="ABN" schemeAgencyID="ato.gov.au" schemeAgencyName="Australian Taxation Office" schemeVersionID=" " schemeDataURI="http://abr.business.gov.au/abrxmlsearchRPC/Forms/SearchByABNv201408.aspx" schemeURI="urn:oasis:names:tc:ebcore:partyid-type:iso6523:0151"> 51083392303</cbc:ID> </cac:PartyIdentification>```  | ```"PartyIdentification":{"ABN":"51083392303"}``` | JSON Implementation maintains a normative code-list of unique short names (eg "ABN") for each identifier category (eg party) |
|IndicatorType  |```<cbc:CopyIndicator foprmat="text"> false</cbc:CopyIndicator>```  | ```"CopyIndicator":false``` |  JSON representation will always use the JSON boolean type|
|MeasureType  |```<cbc:GrossWeightMeasure unitCode="KGM" unitCodeListVersionID="urn:un:unece:uncefact:codelist:specification:66411:8e">130 </cbc:GrossWeightMeasure>```  | ```"GrossWeightMeasure":{"value":130, "UnitCode":"KGM"}``` | JSON representation will be as a value/unit object tuple and, like UBL XML, will always use UN/ECE Rec20 code list |
|NumericType  |```<cbc:CopiesNumeric format="decimal">1.0 </cbc:CopiesNumeric>```  | ```"CopiesNumeric":1.0``` |JSON representation will always use the JSON Number type  |
|QuantityType  |```<cbc:InvoicedQuantity unitCode="KG" unitCodeListID="urn:un:unece:uncefact:codelist:specification:66411:8e" unitCodeListAgencyID="UN/ECE" unitCodeListAgencyName="United Nations Economic Commission for Europe">100 </cbc:InvoicedQuantity>```  | ```"InvoicedQuantity":{"Quantity":100, "UnitCode":"KGM"```} |JSON representation will be as a value/unit object tuple and, like UBL XML, will always use UN/ECE Rec20 code list  |
|TextType  | ```<cbc:Name languageID="EN" languageLocaleID="AU">ACME Pty Ltd</cbc:Name>``` |```"Name":"ACME Pty Ltd"```  | JSON representation will not specify language for every string data element.  Instead it will use the UBL document level language indicator "Language":"EN" using ISO 639-1 language codes|

