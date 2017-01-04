# Validation API Specifications

The validation API specification defiens a standard interface for the validation of UBL JSON instances at three levels

* Structure : that the JSON instance conforms to the correspondng JSON schema for the specific document type as referenced by the "$schema" element in the JSON instance.
* Codes : that the code values used in the JSON instance are defined in a relevant code-list, which may be either a core code-list or a context specific code list.
* Rules : that the JSON instance complies with the specific business rules for the implementation context identified by the "CustomizationID" element in the JSON instance.

The JSON instances MUST comply with the [UBL JSON Syntax specification](https://github.com/ausdigital/ubl-json/blob/master/docs/JSONSyntax.md).

## API Validator Specification

Is maintained at swaggerhub : **[UBL-JSON APIs](https://app.swaggerhub.com/api/ausdigital/ubl-json/1.0.0)**

The API is very simple - it is the error codes and messages that are the heart of the validation model. All error responses will comply with the ausdigital standard [RESTful Errors structure](https://app.swaggerhub.com/domains/ausdigital/ErrorModel/1.0).  

## Document Structure Error Response Codes


|Error Code | Error Message|
|-----------|--------------|
|jsonval-01 | The $schema reference is missing and so this document cannot be validated  |
|jsonval-02 |   |
|jsonval-03 |   |

## Code-List Error Response Codes

Are maintained together with the [code-lists specification](http://code-lists.readthedocs.io/en/latest/) 

## Business Rules Error Response Codes

Are maintianed together with the relevant semantic specification - for example [billing seamntics](http://billing-semantics.readthedocs.io/en/latest/)



