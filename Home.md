# AMRP API Platform Design Standards

- [AMRP API Platform Design Standards](#amrp-api-platform-design-standards)
- [Introduction](#introduction)
- [Principles](#principles)
    - [API design principles](#api-design-principles)
    - [API as a product](#api-as-a-product)
    - [API first](#api-first)
- [General Guidelines](#general-guidelines)
  - [Document Semantics, Formatting, and Naming](#document-semantics-formatting-and-naming)
  - [**MUST** design the API first](#must-design-the-api-first)
  - [**MUST** provide API specification using OpenAPI](#must-provide-api-specification-using-openapi)
  - [**SHOULD** provide API user manual](#should-provide-api-user-manual)
  - [**MUST** write APIs using U.S. English](#must-write-apis-using-us-english)
- [REST : URLs](#rest--urls)
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
  - [**MUST NOT** pass sensitive information in the URL](#must-not-pass-sensitive-information-in-the-url)
  - [**SHOULD NOT** use null for query parameters](#should-not-use-null-for-query-parameters)
- [REST : Security](#rest--security)
  - [**MUST** secure endpoints](#must-secure-endpoints)
  - [# REST : Payloads](#-rest--payloads)
  - [**MUST** support JSON as payload data interchange format](#must-support-json-as-payload-data-interchange-format)
  - [**MUST** support JSON in UTF-8](#must-support-json-in-utf-8)
  - [**MUST** use JSON payloads conatianing only valid Unicode strings](#must-use-json-payloads-conatianing-only-valid-unicode-strings)
  - [**MUST** use JSON payloads consisting of unique member names](#must-use-json-payloads-consisting-of-unique-member-names)
  - [**MAY** pass non-JSON media types using data specific standard formats](#may-pass-non-json-media-types-using-data-specific-standard-formats)
  - [**SHOULD** not use null for empty arrays](#should-not-use-null-for-empty-arrays)
  - [# REST : HTTP Requests](#-rest--http-requests)
- [**MUST** use HTTP methods correctly](#must-use-http-methods-correctly)
  - [**MAY** use overloaded POST](#may-use-overloaded-post)
  - [**MUST** design GET, PUT, HEAD and OPTIONS to be idempotent](#must-design-get-put-head-and-options-to-be-idempotent)
  - [**SHOULD** design POST, DELETE and PATCH idempotent](#should-design-post-delete-and-patch-idempotent)
  - [**SHOULD** use a custom idempotency header (Idempotency-Key) to facilitate idempotent POST, DELETE or PATCH](#should-use-a-custom-idempotency-header-idempotency-key-to-facilitate-idempotent-post-delete-or-patch)
  - [**MAY** use overloaded POST in scenarios where GET or DELETE would require passing sensitive information in the URL](#may-use-overloaded-post-in-scenarios-where-get-or-delete-would-require-passing-sensitive-information-in-the-url)
  - [**SHOULD** design simple query languages using query parameters](#should-design-simple-query-languages-using-query-parameters)
  - [**SHOULD** design complex query languages using JSON](#should-design-complex-query-languages-using-json)
- [later](#later)

    
  
  
  
  
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






  

# Principles

### [API design principles](#api-design-principles)

Comparing SOA web service interfacing style of SOAP vs. REST, the former tend to be centered around operations that are usually use-case specific and specialized. In contrast, REST is centered around business (data) entities exposed as resources that are identified via URIs and can be manipulated via standardized CRUD-like methods using different representations, and hypermedia. RESTful APIs tend to be less use-case specific and come with less rigid client / server coupling and are more suitable for an ecosystem of (core) services providing a platform of APIs to build diverse new business services. We apply the RESTful web service principles to all kind of application (micro-) service components, independently from whether they provide functionality via the internet or intranet.

*   We prefer REST-based APIs with JSON payloads
    
*   We prefer systems to be truly RESTful \[1](#_footnotedef_1 "View footnote.")\]
    

An important principle for API design and usage is Postel’s Law, aka [The Robustness Principle](http://en.wikipedia.org/wiki/Robustness_principle) (see also [RFC 1122](https://tools.ietf.org/html/rfc1122)):

*   Be liberal in what you accept, be conservative in what you send
    

_Readings:_ Some interesting reads on the RESTful API design style and service architecture:

*   Article: [REST API Design - Resource Modeling](https://www.thoughtworks.com/insights/blog/rest-api-design-resource-modeling)
    
*   Article: [Richardson Maturity Model — Steps toward the glory of REST](https://martinfowler.com/articles/richardsonMaturityModel.html)
    
*   Book: [Irresistible APIs: Designing web APIs that developers will love](https://www.amazon.de/Irresistible-APIs-Designing-that-developers/dp/1617292559)
    
*   Book: [REST in Practice: Hypermedia and Systems Architecture](http://www.amazon.de/REST-Practice-Hypermedia-Systems-Architecture/dp/0596805829)
    
*   Book: [Build APIs You Won’t Hate](https://leanpub.com/build-apis-you-wont-hate)
    
*   Fielding Dissertation: [Architectural Styles and the Design of Network-Based Software Architectures](http://www.ics.uci.edu/~fielding/pubs/dissertation/top.htm)
    

### [API as a product](#api-as-a-product)

As a company we want to deliver products to our (internal and external) clients which can be consumed like a service. Embracing 'API as a Product' facilitates a service ecosystem, which can be evolved more easily and used to experiment quickly with new business ideas by recombining core capabilities. It makes the difference between agile, innovative product service business built on a platform of APIs and ordinary enterprise integration business where APIs are provided as "appendix" of existing products to support system integration and optimised for local server-side realization.

Platform products provide their functionality via (public) APIs; hence, the design of our APIs should be based on the API as a Product principle:

*   Treat your API as product and act like a product owner
    
*   Put yourself into the place of your user; be an advocate for their needs
    
*   Emphasize simplicity, comprehensibility, and usability of APIs to make them irresistible for client engineers
    
*   Actively improve and maintain API consistency over the long term
    
*   Make use of customer feedback and provide service level support

*   Understand the concrete use cases of your customers and carefully check the trade-offs of your API design variants with a product mindset. 

*   Avoid short-term implementation optimizations, 

*   Have a high attention on API quality and client developer experience.
  

API as a Product is closely related to our [API First principle](#100) (see next chapter) which is more focused on how we engineer high quality APIs.

### [API first](#api-first)

API First is one of our core requirements. In a nutshell API First requires two aspects:

*   define APIs first, before coding its implementation, using a standard specification language (OpenAPI)
    
*   get early review feedback from peers and client developers
    

By defining APIs outside the code, we want to facilitate early review feedback and also a development discipline that focus service interface design on…​

*   profound understanding of the domain and required functionality
    
*   generalized business entities / resources, i.e. avoidance of use case specific APIs
    
*   clear separation of WHAT vs. HOW concerns, i.e. abstraction from implementation aspects — APIs should be stable even if we replace complete service implementation including its underlying technology stack
    

Moreover, API definitions with standardized specification format also facilitate…​

*   single source of truth for the API specification; it is a crucial part of a contract between service provider and client users
    
*   infrastructure tooling for API discovery, API GUIs, API documents, automated quality checks
    

A mandatory element of the API First principle is the API review process. This process facilitates early review and feedback from peers and client developers. Peer review is important for us to get high quality APIs, to enable architectural and design alignment and to supported development of client applications decoupled from service provider engineering life cycle.

It is important to note that API First is **not in conflict with the agile development principles** that we love. Service applications should evolve incrementally — and so should its APIs. Of course, our API specification will and should evolve iteratively in different cycle. API evolution during the development life cycle may include breaking changes for not yet productive features. Hence, API First does _not_ mean that you must have 100% domain and requirement understanding and can never produce code before you have defined the complete API and get it confirmed by peer review.

On the other hand, API First obviously is in conflict with the bad practice of publishing API definition and asking for peer review after the service integration or even the services productive operation has started. It is crucial to request and get early feedback — as early as possible, but not before the API changes are comprehensive with focus to the next evolution step and have a certain quality (including API Guideline compliance), already confirmed via team internal reviews.



# General Guidelines

## Document Semantics, Formatting, and Naming
The keywords "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in RFC 2119.

The words "REST" and "RESTful" MUST be written as presented here, representing the acronym as all upper-case letters. This is also true of "JSON," "XML," and other acronyms.

Machine-readable text, such as URLs, HTTP verbs, and source code, are represented using a fixed-width font.

URIs containing variable blocks are specified according to URI Template RFC 6570. For example, a URL containing a variable called account_id would be shown as https://foo.com/accounts/{account_id}.

HTTP headers are written in title case + hyphenated syntax, e.g. Foo-Request-Id.


## **MUST** design the API first

You must follow the [API First Principle](#api-first), more specifically:

*   You must define APIs first, before coding its implementation, [using OpenAPI as specification language](#101)
    
*   You must design your APIs consistently with these guidelines; 
    
*   You must call for early review feedback from peers and client developers for all component external facing APIs



## **MUST** provide API specification using OpenAPI


We use the [OpenAPI specification](http://swagger.io/specification/) as standard to define API specification files. API designers are required to provide the API specification using a single **self-contained YAML** file to improve readability. We encourage to use **OpenAPI 3.0** version, but still support **OpenAPI 2.0** (a.k.a. Swagger 2).

The API specification files should be subject to version control using a source code management system - together with the implementing sources.

You must publish the component  API specification with the deployment of the implementing service, and, hence, make it discoverable.

**Hint:** A good way to explore **OpenAPI 3.0/2.0** is to navigate through the [OpenAPI specification mind map](https://openapi-map.apihandyman.io/).


## **SHOULD** provide API user manual

In addition to the API Specification, it is good practice to provide an API user manual to improve client developer experience, especially of engineers that are less experienced in using this API. A helpful API user manual typically describes the following API aspects:

*   API scope, purpose, and use cases
    
*   concrete examples of API usage
    
*   edge cases, error situation details, and repair hints
    
*   architecture context and major dependencies - including figures and sequence flows
    

The user manual must be published online. Please do not forget to include a link to the API user manual into the API specification using the `#/externalDocs/url` property.

## **MUST** write APIs using U.S. English
All APIs must be written in U.S. English to keep the language and spelling consistent

# REST : URLs

## **MUST** pluralize resource names that are collections
  The majority of resources available in a RESTful API are collections. These should be pluralized to indicate they are access points to many resources

  Example : 

    Incorrect : /collector/{id}
    Correct   : /collectors/{id}


  In cases where the resource is a singular entity, the pluralization may be dropped.

  Example : 

      /collectors/{id}/config
      
  Here we have a one to one relationship between the collector and their config. In this case we do not need to pluralize. If the collector had more than one conifg, then we would need to pluralize like `/collectors/{id}/configs/{cid}`

  

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


## **MUST NOT** pass sensitive information in the URL
  
  We must not pass data the could be considered sensitive (e.g. Personal Information (PI) like credit card numbers, user name etc) in the URLS since it will be exposed to various caching and logging technologies (browser cache, CDN, Splunk etc). 

  ***see overloaded POST for solutions

## **SHOULD NOT** use null for query parameters
  We should not use null values as input or state indicators. Instead we should either build up a domain specific language, or use a null object pattern to indicate the absense of a value.

  *Incorrect* : `api.airmiles.ca/collectors/{id}/segments?type=null`
                    
  *Correct*   : `api.airmiles.ca/collectors-experience/1234?type=none` 




# REST : Security 

  ## **MUST** secure endpoints
  Every API endpoint must be protected and armed with authentication and authorization. As part of the API definition you must specify how you protect your API using either the `http` typed `bearer` or `oauth2` typed security schemes defined in the [OpenAPI Authentication Specification](https://swagger.io/docs/specification/authentication/).




  ## **MUST** not expose stack traces
  Often error states can expose stack traces that can become a security concer by exposing server and application data that can be used as clues on how to compromise the server. To avoid revealing this type of information, the application must not expose stack traces in any production environment. 

  #TODO : Add guidance on techniques to avoid this

  
# REST : Payloads
--------------------
These guidelines provides recommendations for defining JSON data at LoyatyOne. JSON here refers to [RFC 7159](https://tools.ietf.org/html/rfc7159) the "application/json" media type and custom JSON media types defined for APIs. The guidelines clarifies some specific cases to allow LoyatyOne JSON data to have an idiomatic form across teams and services.

  ## **MUST** support JSON as payload data interchange format

  Use JSON ([RFC 7159](https://tools.ietf.org/html/rfc7159)) to represent structured (resource) data passed with HTTP requests and responses as body payload. The JSON payload must use a JSON object as top-level data structure (if possible) to allow for future extension. This also applies to collection resources, where you ad-hoc would use an array — see also [**MUST** always return JSON objects as top-level data structures](#110).

  Additionally, the JSON payload must comply to the more restrictive Internet JSON ([RFC 7493](https://tools.ietf.org/html/rfc7493)), particularly

  *   [Section 2.1](https://tools.ietf.org/html/rfc7493#section-2.1) on encoding of characters, and
        
  *   [Section 2.3](https://tools.ietf.org/html/rfc7493#section-2.3) on object constraints.

  As a consequence, a JSON payload must

   * [**MUST** support JSON in UTF-8](#must-support-json-in-utf-8)
   * [**MUST** use JSON payloads conatianing only valid Unicode strings](#must-use-json-payloads-conatianing-only-valid-unicode-strings)
   * [**MUST** use JSON payloads consisting of unique member names](#must-use-json-payloads-consisting-of-unique-member-names)

  


  ## **MUST** support JSON in UTF-8

  As a result of supporting Internet JSON ([RFC 7493](https://tools.ietf.org/html/rfc7493)), a JSON payload must use [`UTF-8` encoding](https://tools.ietf.org/html/rfc7493#section-2.1)

  ## **MUST** use JSON payloads conatianing only valid Unicode strings
  
  As a result of supporting Internet JSON ([RFC 7493](https://tools.ietf.org/html/rfc7493)), a JSON payload must consist of [valid Unicode strings](https://tools.ietf.org/html/rfc7493#section-2.1), i.e. must not contain non-characters or surrogates, and

  ## **MUST** use JSON payloads consisting of unique member names
  As a result of supporting Internet JSON ([RFC 7493](https://tools.ietf.org/html/rfc7493)), a JSON payload must contain only [unique member names](https://tools.ietf.org/html/rfc7493#section-2.3) (no duplicate names).

  ## **MAY** pass non-JSON media types using data specific standard formats

  Non-JSON media types may be supported, if you stick to a business object specific standard format for the payload data, for instance, image data format (JPG, PNG, GIF), document format (PDF, DOC, ODF, PPT, CSV), or archive format (TAR, ZIP).

  Generic structured data interchange formats other than JSON (e.g. XML, BSON) may be provided, but only additionally to JSON as default format using content negotiation, for specific use cases where clients may not interpret the payload structure.
  
  ## **SHOULD** not use null for empty arrays
  Empty array values can unambiguously be represented as the empty list, [].

  

# REST : HTTP Requests
-------------------------
  ## **MUST** use HTTP methods correctly

  Be compliant with the standardized HTTP method semantics (see HTTP/1 [RFC-7230](https://tools.ietf.org/html/rfc7230) and [RFC-7230](https://tools.ietf.org/html/rfc7231) updates from 2014) summarized as follows:

  | HTTP Method | Description                                                                |
  | ----------- | -------------------------------------------------------------------------- |
  | `GET`       | To _retrieve_ a resource.                                                  |
  | `POST`      | To _create_ a resource, or to _execute_ a complex operation on a resource. |
  | `PUT`       | To _update_ a resource.                                                    |
  | `DELETE`    | To _delete_ a resource.                                                    |
  | `PATCH`     | To perform a _partial update_ to a resource.                               |  


  ## **MAY** use overloaded POST
  

  We should always make our absolute best effort to keep in the spirit of REST, and use the REST VERBs as intended. 
  
  Avoid the techique below if at all possible 

  In some very specific scenarios it might be not be possible to use the REST verbs as intended. A good example is a bodyless requests verbs like GET or DELETE may not be able to avoid requiring a request body. One scenario is when it is not allowable to expose sensitive information (e.g. PI, credit card numbers etc) within the URL since it will be exposed to various caching and logging technologies (browser cache, CDN, Splunk etc). Or the query parameters could exceed size limits of a piece of technology in the stack (e.g. clients, load-balancers, and servers). 

  In cases like this it is advised to use overloaded POST as a technique to overcome this limitation. 
  
  In this case we use the POST VERB for a GET or DELETE operation, and make sure we explicitly note the exception in our documentation like so :

    paths:
      /collectors:
        post:
          description: >
            [GET with body payload](https://github.com/LoyaltyOne/API-Guidelines#may-use-overloaded-post) - no resources created:
            Returns all collectors matching the query passed as request input payload.
          requestBody:
            required: true
            content:
    

  ## **MUST** design GET, HEAD, OPTIONS, TRACE to be safe

   An operation is considered [safe](https://tools.ietf.org/html/rfc7231#section-4.2.1) if 
   
      "if their defined semantics are essentially read-only; i.e., the client does not request, and does not expect, any state change on the origin server as a result of applying a safe method to a target resource"

   For example, a GET request should never have any side effect visible to the client like updating the the state of the resource. 


  ## **MUST** design GET, PUT, DELETE, HEAD and OPTIONS to be idempotent
  
  An operation is considered [idempotent](https://tools.ietf.org/html/rfc7231#section-4.2.2) 
      
      "if the intended effect on the server of multiple identical requests with that method is the same as the effect for a single such request."

  For example multiple requests to DELETE the same resource should not cause an error.
    
  ## **SHOULD** design POST, DELETE and PATCH idempotent

  In many cases it is helpful or even necessary to design `POST` and `PATCH` idempotent for clients to expose conflicts and prevent resource duplicate (a.k.a. zombie resources) or lost updates
  
  For example, if same resources may be created or changed in parallel or multiple times via `POST`, using idempotent `POST` is recommended. 
  
  ## **SHOULD** design simple query languages using query parameters
  We prefer the use of query parameters to describe resource-specific query languages for the majority of APIs because it’s native to HTTP, easy to extend and has excellent implementation support in HTTP clients and web frameworks.

  Query parameters should have the following aspects specified:

  *   Reference to corresponding property, if any
      
  *   Value range, e.g. inclusive vs. exclusive
      
  *   Comparison semantics (equals, less than, greater than, etc)
      
  *   Implications when combined with other queries, e.g. _and_ vs. _or_
      

  How query parameters are named and used is up to individual API designers. The following examples should serve as ideas:

  *   `name=LoyatyOne`, to query for elements based on property equality
      
  *   `age=5`, to query for elements based on logical properties
      
      *   Assuming that elements don’t actually have an `age` but rather a `birthday`
          
      
  *   `max_length=5`, based on upper and lower bounds (`min` and `max`)
      
  *   `shorter_than=5`, using terminology specific e.g. to _length_
      
  *   `created_before=2019-07-17` or `not_modified_since=2019-07-17`

  We don’t advocate for or against certain names because in the end APIs should be free to choose the terminology that fits their domain the best.

  ## **SHOULD** design complex query languages using JSON

  Minimalistic query languages based on [query parameters](#236) are suitable for simple use cases with a small set of available filters that are combined in one way and one way only (e.g. _and_ semantics). Simple query languages are generally preferred over complex ones.

  Some APIs will have a need for sophisticated and more complex query languages. Dominant examples are APIs around search (incl. faceting) and product catalogs.

  Aspects that set those APIs apart from the rest include but are not limited to:

  *   Unusual high number of available filters
      
  *   Dynamic filters, due to a dynamic and extensible resource model
      
  *   Free choice of operators, e.g. `and`, `or` and `not`
      

  APIs that qualify for a specific, complex query language are encouraged to use nested JSON data structures and define them using OpenAPI directly. This provides the following benefits:

  *   Data structures are easy to use for clients
      
      *   No special library support necessary
          
      *   No need for string concatenation or manual escaping
          
      
  *   Data structures are easy to use for servers
      
      *   No special tokenizers needed
          
      *   Semantics are attached to data structures rather than text tokens
          
      
  *   Consistent with other HTTP methods
      
  *   API is defined in OpenAPI completely
      
      *   No external documents or grammars needed
          
      *   Existing means are familiar to everyone
        
    

[JSON-specific rules](#json-guidelines) and most certainly needs to make use of the [`GET`\-with-body](#get-with-body) pattern.


# HTTP Status Codes
  ## **MUST** use official HTTP status codes 
  ## **SHOULD** only use most common HTTP status codes
  ## **MUST** use most specific HTTP status codes
  


# later
MUST use snake_case (never camelCase) for query parameters
https://gist.github.com/azhawkes/3db84b194b3e47423df2



References

https://www.ics.uci.edu/~fielding/pubs/dissertation

https://docs.microsoft.com/en-us/azure/architecture/best-practices/api-design

https://cloud.google.com/apis/design

https://opensource.zalando.com/restful-api-guidelines

https://martinfowler.com/articles/richardsonMaturityModel.html

