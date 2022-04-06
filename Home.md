# AMRP API Platform Design Standards

- [AMRP API Platform Design Standards](#amrp-api-platform-design-standards)
- [Introduction](#introduction)
- [Document Semantics, Formatting, and Naming](#document-semantics-formatting-and-naming)
- [General Guidlines](#general-guidlines)
  - [**Must** design the API first (#api-first-principle)](#must-design-the-api-first-api-first-principle)
  - [**MUST** pluralize resource names that are collections](#must-pluralize-resource-names-that-are-collections)
  - [**MUST** use kebab-case for compound words in path segments](#must-use-kebab-case-for-compound-words-in-path-segments)
  - [**MUST** use normalized paths without empty path segments and trailing slashes](#must-use-normalized-paths-without-empty-path-segments-and-trailing-slashes)
  - [**MUST** keep URLs verb-free](#must-keep-urls-verb-free)
  - [**MUST** identify resources and sub-resources via path segments](#must-identify-resources-and-sub-resources-via-path-segments)
  - [**MAY** consider using nested URLs for dependant resources](#may-consider-using-nested-urls-for-dependant-resources)
  - [**SHOULD** limit number of resource types](#should-limit-number-of-resource-types)
  - [**SHOULD** limit number of sub-resource levels](#should-limit-number-of-sub-resource-levels)
  - [**MUST** use snake_case for query parameters](#must-use-snake_case-for-query-parameters)
  - [**MUST** stick to conventional query parameters](#must-stick-to-conventional-query-parameters)
 
  -
MUST write APIs using English
  - URL's    
    



    
    

  - Security 
    - **MUST** secure endpoints


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

The purpose of this document is to ensure consistency and the standard application of practices when designing and implementing API's at AMRP. It details the requirements, conventions and best practices on how to create an API that is maintainable, extensible, discoverable and consistent. This facilitates a great developer experience and the ability to quickly compose business value.

To this end, our API's should be 

- easy to learn and understand
- are generalized, and solve more than a specific business usecase
- have a common look and feel 
- are RESTful in accordance with Level 2 in the Richarson maturity model   
- should be treated like a product, with the developing team acting like a product owner and user


Further we recoginize that we are dealing with existing implementations. To this end when guidlines are changing, the following rules apply:

- existing APIs don’t have to be changed, but we recommend it
- clients of existing APIs have to cope with these APIs based on outdated rules
- new APIs have to respect the current guidelines


Please note, all example URL's used in this document are fictitous. They do not really exists and are used here only for illustrative purposes




# Document Semantics, Formatting, and Naming
The keywords "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in RFC 2119.

The words "REST" and "RESTful" MUST be written as presented here, representing the acronym as all upper-case letters. This is also true of "JSON," "XML," and other acronyms.

Machine-readable text, such as URLs, HTTP verbs, and source code, are represented using a fixed-width font.

URIs containing variable blocks are specified according to URI Template RFC 6570. For example, a URL containing a variable called account_id would be shown as https://foo.com/accounts/{account_id}.

HTTP headers are written in title case + hyphenated syntax, e.g. Foo-Request-Id.

  

# General Guidlines


## **Must** design the API first (#api-first-principle)
Before starting any coding of a RESTful service, the API interface must be designed first using OpenAPI. 

By focusing on the interface design first we
- make our interface the primary concern, and treat it as a first class operator
- create a single source of truth for the API specification
- give the opportunity for early review from all technichal stakeholders


## **MUST** pluralize resource names that are collections
      Resources that indicate a collection of resources, must be pluralized using an 's'

        *Incorrect* : `api.airmiles.ca/collector/1234` 
        
        *Correct*   : `api.airmiles.ca/collectors/1234`

## **MUST** use kebab-case for compound words in path segments
      We use kabab casining (e.g. kebab-case) to make our URL's more readable. Although there are many ways of making URL's more readable, for example using camel casing (e.g. camelCase) and title casing (e.g. TitleCase), they have their drawbacks. In particular, although URL's in browsers are case insensitive, a number of caching mechanism's both at the CDN and hardware level are not. This makes using kebab case the easiest and most reliable way of insuring human readble URL's with consistency of experience for the user

      *Incorrect* : `api.airmiles.ca/CollectorExperience/1234`
                    `api.airmiles.ca/collectorExperience/1234` 
        
      *Correct*   : `api.airmiles.ca/collectors-experience/1234`  

## **MUST** use normalized paths without empty path segments and trailing slashes
      Resources must not end with a slash

      *Incorrect* : `api.airmiles.ca/collectors/1234/` 
                    `api.airmiles.ca/collectors/1234//`
                    `api.airmiles.ca/collectors///1234//`
      
      *Correct*   : `api.airmiles.ca/collectors/1234`

## **MUST** keep URLs verb-free
      Verbs must not appear in URL's. REST URL's define resources, not actions. Instead actions are indicated as part of the HTTP method. 

      *Incorrect* : `GET api.airmiles.ca/collectors/search`                       
        
      *Correct*   : `GET api.airmiles.ca/collectors/searches`


## **MUST** identify resources and sub-resources via path segments
If a resource can not exists without a parent resource, this relation must be represented as a sub-resource. A sub-resource should be indicated by its name and identifier in the path segment. 

      *Example*   : `GET api.airmiles.ca/shopping-cart/1234/cart-item/10`

## **MAY** consider using nested URLs for dependant resources
If a resource may exists without a parent resource, this relation may be represented as a sub-resource. 

*Example*   : `api.airmiles.ca/collectors/1/orders/123`
              `api.airmiles.ca/orders/123`


## **SHOULD** limit number of resource types
As a general rule, having more than 3-4 resources levels is a good indication that the problem space is too large, and we are violating the single responsabiltiy principle. A resource level is defined as any top level resource in the domain


*Example*   : `api.airmiles.ca/collectors/{id}`
              `api.airmiles.ca/collectors/{id}}/orders/{oid}`
              `api.airmiles.ca/collectors/segment`
              `api.airmiles.ca/orders/{id}}`
              

Here we have 3 resources. One for collector, one for collector orders, and one for orders. We do not consider the segment resource as a new resource because there is a one to one relation between collectors and segments.


## **SHOULD** limit number of sub-resource levels
    As the number of resource in a the path increases, the scope of the domain as well as the complexity increases. To minimize this we should try to keep sub resource depth to a maximum of 3 level

        *Good* : `api.airmiles.ca/collector/{id}/orders/{oid}/item/{iid}`
        *Bad*  : `api.airmiles.ca/collector/{id}/orders/{oid}/item/{iid}/supplier/{sid}/addresses/{aid}}`


## **MUST** use snake_case for query parameters
    We must use snake case for query parameters. This helps avoid various techniologies that are case sensitive in the web ecosystem

      *Example* : `api.airmiles.ca/collector/1234?order_id=1234` 


 
## **MUST** stick to conventional query parameters
      If you provide query support for searching, sorting, filtering, and paginating, you must stick to the following naming conventions:

      q: default query parameter, e.g. used by browser tab completion; should have an entity specific alias, e.g. sku.

      sort: comma-separated list of fields (as defined by MUST define collection format of header and query parameters) to define the sort order. To indicate sorting direction, fields may be prefixed with + (ascending) or - (descending), e.g. /sales-orders?sort=+id.

      offset: numeric offset of the first element provided on a page representing a collection request. 
    
      limit: client suggested limit to restrict the number of entries on a page.

- **MUST NOT** pass sensitive information in the URL
- **SHOULD NOT** use null for query parameters
MUST use snake_case (never camelCase) for query parameters
https://gist.github.com/azhawkes/3db84b194b3e47423df2



| HTTP Method | Description                                                                |
| ----------- | -------------------------------------------------------------------------- |
| `GET`       | To _retrieve_ a resource.                                                  |
| `POST`      | To _create_ a resource, or to _execute_ a complex operation on a resource. |
| `PUT`       | To _update_ a resource.                                                    |
| `DELETE`    | To _delete_ a resource.                                                    |
| `PATCH`     | To perform a _partial update_ to a resource.                               |







References

https://www.ics.uci.edu/~fielding/pubs/dissertation

https://docs.microsoft.com/en-us/azure/architecture/best-practices/api-design

https://cloud.google.com/apis/design

https://opensource.zalando.com/restful-api-guidelines

https://martinfowler.com/articles/richardsonMaturityModel.html
