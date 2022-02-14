# AMRP API Platform Design Standards

- [AMRP API Platform Design Standards](#amrp-api-platform-design-standards)
- [Introduction](#introduction)
- [Preamble](#preamble)
- [Document Semantics, Formatting, and Naming](#document-semantics-formatting-and-naming)
- [General Guidlines](#general-guidlines)

MUST write APIs using English
  - URL's
    - **MUST** pluralize resource names that are collections
    - **MUST** use kebab-case for compound words in path segments
    - **MUST** use normalized paths without empty path segments and trailing slashes
    - **MUST** keep URLs verb-free
    - **MUST** identify resources and sub-resources via path segments
    - **MAY** expose compound keys as resource identifiers???
    - **MAY** consider using (non-) nested URLs
    - **SHOULD** limit number of resource types???
    - **SHOULD** limit number of sub-resource levels
    - **MUST** use snake_case (never camelCase) for query parameters
    - **MUST** use domain-specific resource names
    - **MUST** stick to conventional query parameters
    - **MUST NOT** pass sensitive information in the URL
    - **SHOULD NOT** use null for query parameters

  - Payloads
    - **MUST** support JSON as payload for domain objects
    - **MAY** pass non-JSON media types using data specific standard formats
    - **SHOULD** not use null for empty arrays
    
  - HTTP Requests
    - **MUST** use HTTP methods correctly
    - **MUST** design GET, PUT, HEAD and OPTIONS to be idempotent
    - **SHOULD** design POST, DELETE and PATCH idempotent
    - **SHOULD** use a custom idempotency header (Idempotency-Key) to facilitate idempotent POST, DELETE or PATCH
    - **MAY** use overloaded POST in scenarios where GET or DELETE would require passing sensitive information in the URL
    - **SHOULD** design simple query languages using query parameters
    - **SHOULD** design complex query languages using JSON
  
  - HTTP Status Codes
    - **MUST** use official HTTP status codes 
    - **SHOULD** only use most common HTTP status codes
    - **MUST** use most specific HTTP status codes
    - **MUST** not expose stack traces
  
  - HTTP Headers
    - **MAY** use standard headers
    - **SHOULD** use kebab-case with uppercase separate words for HTTP headers
    - **MAY** consider to support Idempotency-Key header
    - **MUST** propagate proprietary headers
    - **SHOULD** use only the specified proprietary AMRP headers (provide list)
  
  - REST Compliance
    - **MUST** use REST maturity level 2
      - **MUST** avoid actions — think about resources
      - **MUST** keep URLs verb-free
      - **MUST** use HTTP methods correctly
      - **SHOULD** only use most common HTTP status codes
    - **MAY** use REST maturity level 3
  
  - Performance
    - **SHOULD** reduce bandwidth needs and improve responsiveness
    - **SHOULD** use gzip compression
  
  - Compatibility
    - **MUST** not break backward compatibility
    - **SHOULD** prefer compatible extensions
     

# Introduction

The purpose of this document is to ensure consistency and the standard application of practices when designing and implementing API's at AMRP. It details the requirements, conventions and best practices on how to create an API that is maintainable, extensible, discoverable and consistent. This facilitates a great developer experience and the ability to quickly compose business processes.  

# Preamble
AMRP's software architecture is designed to encapsulate small pieces of business value in  microservices that expose functionality via RESTful APIs with a JSON payloads. In no other part of the organization are business capabilities expressed as precisely and fully as in our API's. As such, our API's our highly valuable business assets, both as the interface into our business capabilities, and as living documentation of the value we are delivering.

This makes designing high quality, long lasting API's a mission critical endeavor to the organization of the utmost importance. To facilitate creating API's that fulfill the above criteria, this living document must be used to assure a consistent, durable approach to designing API's within Airmiles 

# Document Semantics, Formatting, and Naming
The keywords "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in RFC 2119.

The words "REST" and "RESTful" MUST be written as presented here, representing the acronym as all upper-case letters. This is also true of "JSON," "XML," and other acronyms.

Machine-readable text, such as URLs, HTTP verbs, and source code, are represented using a fixed-width font.

URIs containing variable blocks are specified according to URI Template RFC 6570. For example, a URL containing a variable called account_id would be shown as https://foo.com/accounts/{account_id}.

HTTP headers are written in title case + hyphenated syntax, e.g. Foo-Request-Id.


# General Guidlines

[**MUST** follow API first principle](#api-first-principle)


MUST provide API specification using OpenAPI

SHOULD provide API user manual

MUST write APIs using English


MUST use snake_case (never camelCase) for query parameters
https://gist.github.com/azhawkes/3db84b194b3e47423df2



|HTTP Method|Description|
|---|---|
| `GET`| To _retrieve_ a resource. |
| `POST`| To _create_ a resource, or to _execute_ a complex operation on a resource. |
| `PUT`| To _update_ a resource. |
| `DELETE`| To _delete_ a resource. |
| `PATCH`| To perform a _partial update_ to a resource. |







References
https://www.ics.uci.edu/~fielding/pubs/dissertation/
https://docs.microsoft.com/en-us/azure/architecture/best-practices/api-design
https://cloud.google.com/apis/design
https://opensource.zalando.com/restful-api-guidelines
