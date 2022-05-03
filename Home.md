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
- [URLs](#urls)
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
- [Security](#security)
  - [**MUST** secure endpoints](#must-secure-endpoints)
  - [**MUST** not expose stack traces](#must-not-expose-stack-traces)
  - [# Payloads](#-payloads)
  - [**MUST** support JSON as payload data interchange format](#must-support-json-as-payload-data-interchange-format)
  - [**MUST** support JSON in UTF-8](#must-support-json-in-utf-8)
  - [**MUST** use JSON payloads conatianing only valid Unicode strings](#must-use-json-payloads-conatianing-only-valid-unicode-strings)
  - [**MUST** use JSON payloads consisting of unique member names](#must-use-json-payloads-consisting-of-unique-member-names)
  - [**MAY** pass non-JSON media types using data specific standard formats](#may-pass-non-json-media-types-using-data-specific-standard-formats)
  - [**SHOULD** not use null for empty arrays](#should-not-use-null-for-empty-arrays)
  - [# HTTP Requests](#-http-requests)
  - [**MUST** use HTTP methods correctly](#must-use-http-methods-correctly)
  - [**MAY** use overloaded POST](#may-use-overloaded-post)
  - [**MUST** design GET, HEAD, OPTIONS, TRACE to be safe](#must-design-get-head-options-trace-to-be-safe)
  - [**MUST** design GET, PUT, DELETE, HEAD and OPTIONS to be idempotent](#must-design-get-put-delete-head-and-options-to-be-idempotent)
  - [**SHOULD** design POST, DELETE and PATCH idempotent](#should-design-post-delete-and-patch-idempotent)
  - [**SHOULD** design simple query languages using query parameters](#should-design-simple-query-languages-using-query-parameters)
  - [**SHOULD** design complex query languages using JSON](#should-design-complex-query-languages-using-json)
- [HTTP Status Codes](#http-status-codes)
  - [**MUST** use official HTTP status codes](#must-use-official-http-status-codes)
  - [**SHOULD** only use most common HTTP status codes](#should-only-use-most-common-http-status-codes)
    - [Success codes](#success-codes)
    - [Redirection codes](#redirection-codes)
    - [Client side error codes](#client-side-error-codes)
    - [Server side error codes:](#server-side-error-codes)
  - [**MUST** use most specific HTTP status codes](#must-use-most-specific-http-status-codes)
- [HTTP Headers](#http-headers)
  - [**MAY** use standard headers](#may-use-standard-headers)
  - [**SHOULD** use kebab-case with uppercase separate words for HTTP headers](#should-use-kebab-case-with-uppercase-separate-words-for-http-headers)
  - [**MAY** consider to support Idempotency-Key header](#may-consider-to-support-idempotency-key-header)
  - [**SHOULD** use only the specified proprietary AMRP headers (provide list)](#should-use-only-the-specified-proprietary-amrp-headers-provide-list)
  - [**MUST** propagate proprietary headers](#must-propagate-proprietary-headers)
- [REST Compliance Levels](#rest-compliance-levels)
  - [**MUST** use REST maturity level 2](#must-use-rest-maturity-level-2)
  - [**MAY** use REST maturity level 3 - HATEOAS](#may-use-rest-maturity-level-3---hateoas)
- [Performance](#performance)
  - [**SHOULD** reduce bandwidth needs and improve responsiveness](#should-reduce-bandwidth-needs-and-improve-responsiveness)
  - [**SHOULD** use gzip compression](#should-use-gzip-compression)
  - [**SHOULD** support partial responses via filtering](#should-support-partial-responses-via-filtering)
  - [**SHOULD** support partial responses via filtering](#should-support-partial-responses-via-filtering-1)
    - [Unfiltered](#unfiltered)
    - [Filtered](#filtered)
- [Compatibility and Versioning](#compatibility-and-versioning)
  - [**MUST** not break backward compatibility](#must-not-break-backward-compatibility)
  - [**SHOULD** prefer compatible extensions](#should-prefer-compatible-extensions)
  - [**SHOULD** design APIs conservatively](#should-design-apis-conservatively)
  - [**MUST** prepare clients to accept compatible API extensions](#must-prepare-clients-to-accept-compatible-api-extensions)
  - [**MUST** treat OpenAPI specification as open for extension by default](#must-treat-openapi-specification-as-open-for-extension-by-default)
  - [**SHOULD** avoid versioning](#should-avoid-versioning)
  - [[**MUST** not use URL versioning](#must-not-use-url-versioning)
  - [# Data formats](#-data-formats)
    - [[**SHOULD** use standard data formats \[238\]](#238)](#should-use-standard-data-formats-238)
  - [[**MUST** use standard formats for date and time properties \[169\]](#169)](#must-use-standard-formats-for-date-and-time-properties-169)
  - [[**MUST** use standard formats for country, language and currency properties \[170\]](#170)](#must-use-standard-formats-for-country-language-and-currency-properties-170)
  - [**SHOULD** use content negotiation, if clients may choose from different resource representations](#should-use-content-negotiation-if-clients-may-choose-from-different-resource-representations)
- [References](#references)

    

# [Introduction](#introduction)

The purpose of this document is to ensure consistency and the standard application of practices when designing and implementing API's at AMRP. It details the requirements, conventions and best practices on how to create an API that is maintainable, extensible, discoverable and consistent. This facilitates a great developer experience and the ability to quickly compose business value.

Airmiles software architecture centers around decoupled microservices that provide functionality via RESTful APIs with a JSON payload. Small engineering teams own, deploy and operate these microservices in into AWS. Our APIs most purely express what our systems do, and are therefore highly valuable business assets. Designing high-quality, long-lasting APIs is an integral part of our business strategy that emphasizes developing lots of public APIs for our external business partners to use and build value around.

With this in mind, we’ve adopted "API First" as one of our key engineering principles. Microservices development begins with API definition outside the code and involves ample peer-review feedback to achieve high-quality APIs. API First encompasses a set of quality-related standards and fosters a peer review culture including a lightweight review procedure. We encourage our teams to follow them to ensure that our APIs:

 * are easy to understand and learn

 * are general and abstracted from specific implementation and use cases

 * are robust and easy to use

 * have a common look and feel

 * follow a consistent RESTful style and syntax

 * are consistent with other teams’ APIs and our global architecture

Ideally, all Airmiles APIs will look like the same author created them.

Further we recoginize that we are dealing with existing implementations. To this end when guidlines are changing, the following rules apply:

* existing APIs don’t have to be changed, but we recommend it
* clients of existing APIs have to cope with these APIs based on outdated rules
* new APIs have to respect the current guidelines


**Note :** all example URL's used in this document are fictitous. They do not really exists and are used here only for illustrative purposes


# Principles

### [API design principles](#api-design-principles)

Comparing SOA web service interfacing style of SOAP vs. REST, the former tend to be centered around operations that are usually use-case specific and specialized. In contrast, REST is centered around business (data) entities exposed as resources that are identified via URIs and can be manipulated via standardized CRUD-like methods using different representations, and hypermedia. RESTful APIs tend to be less use-case specific and come with less rigid client / server coupling and are more suitable for an ecosystem of (core) services providing a platform of APIs to build diverse new business services. We apply the RESTful web service principles to all kind of application (micro-) service components, independently from whether they provide functionality via the internet or intranet.

*   We prefer REST-based APIs with JSON payloads
    
*   We prefer systems to be truly RESTful ([maturity level 2](https://martinfowler.com/articles/richardsonMaturityModel.html#level2)
    

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

# URLs

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




# Security 

  ## **MUST** secure endpoints
  Every API endpoint must be protected and armed with authentication and authorization. As part of the API definition you must specify how you protect your API using either the `http` typed `bearer` or `oauth2` typed security schemes defined in the [OpenAPI Authentication Specification](https://swagger.io/docs/specification/authentication/).




  ## **MUST** not expose stack traces

  Stack traces contain implementation details that are not part of an API, and on which clients should never rely. Moreover, stack traces can leak sensitive information that partners and third parties are not allowed to receive and may disclose insights about vulnerabilities to attackers.
  
  #TODO : Add guidance on techniques to avoid this

# Payloads
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

  

# HTTP Requests
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


#  HTTP Status Codes
  ## **MUST** use official HTTP status codes 

  You must only use official HTTP status codes consistently with their intended semantics.  

  Official HTTP status codes are defined via RFC standards and registered in the [IANA Status Code Registry](https://www.iana.org/assignments/http-status-codes/http-status-codes.xhtml). 

  Main RFC standards are [RFC7231 - HTTP/1.1: Semantics](https://tools.ietf.org/html/rfc7231#section-6) (or [RFC7235 - HTTP/1.1: Authentication](https://tools.ietf.org/html/rfc7235#page-6)) and [RFC 6585 - HTTP: Additional Status Codes](https://tools.ietf.org/html/rfc6585)  

  An overview on the official error codes provides [Wikipedia: HTTP status codes](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes) (which also lists some unofficial status codes, e.g. defined by popular web servers like Nginx, that we do not suggest to use).


  ## **SHOULD** only use most common HTTP status codes

   ### [Success codes](#success-codes)

   Code | Meaning | Methods
    -|-|-|
    [200](#status-code-200) | OK - this is the standard success response |  `<all>`
    [201](#status-code-201) | Created - Returned on successful entity creation. You are free to return either an empty response or the created resource in conjunction with the Location header. (More details found in the [\[standard-headers\]](#standard-headers).) _Always_ set the Location header. | [`POST`](#post), `PUT`
    [202](#status-code-202) | Accepted - The request was successful and will be processed asynchronously. | [`POST`](#post), `PUT`, [`PATCH`](#patch), [`DELETE`](#delete)
    [204](#status-code-204) | No content - There is no response body. | `PUT`, [`PATCH`](#patch), [`DELETE`](#delete)
    [207](#status-code-207) |Multi-Status - The response body contains multiple status informations for different parts of a batch/bulk request (see [**MUST** use code 207 for batch or bulk requests](#152)).| [`POST`](#post), ([`DELETE`](#delete))



   ### [Redirection codes](#redirection-codes)

   Code | Meaning | Methods |
    |-|-|-|
    [301](#status-code-301)| Moved Permanently - This and all future requests should be directed to the given URI.|`<all>`
    [303](#status-code-303)|See Other - The response to the request can be found under another URI using a `GET` method.|[`POST`](#post), `PUT`, [`PATCH`](#patch), [`DELETE`](#delete)
    [304](#status-code-304)|Not Modified - indicates that a conditional GET or HEAD request would have resulted in 200 response if it were not for the fact that the condition evaluated to false, i.e. resource has not been modified since the date or version passed via request headers If-Modified-Since or If-None-Match.|`GET`, [`HEAD`](#head)

   ### [Client side error codes](#client-side-error-codes)

   Code | Meaning | Methods |
    |-|-|-|
    [400](#status-code-400) |Bad request - unspecific client error indicating that the server cannot process the request due to something that is perceived to be a client error (e.g. malformed request syntax, invalid request). Should also be delivered in case of input payload fails business logic / semantic validation (instead of using status code 422).|`<all>`
    [401](#status-code-401) | Unauthorized - actually "Unauthenticated": credentials are not valid for the target resource. User must log in. | `<all>`
    [403](#status-code-403) | Forbidden - the user is not authorized to use this resource.|`<all>`
    [404](#status-code-404) | Not found - the resource is not found. |`<all>`
    [405](#status-code-405) | Method Not Allowed - the method is not supported, see [`OPTIONS`](#options).| `<all>`
    [406](#status-code-406) | Not Acceptable - resource can only generate content not acceptable according to the Accept headers sent in the request. | `<all>`
    [408](#status-code-408)| Request timeout - the server times out waiting for the resource. | `<all>`
    [409](#status-code-409) | Conflict - request cannot be completed due to conflict with the current state of the target resource. For instance, in situations where two clients try to create the same resource or if there are concurrent, conflicting updates. | [`POST`](#post), `PUT`, [`PATCH`](#patch), [`DELETE`](#delete)
    [410](#status-code-410) | Gone - resource does not exist any longer, e.g. when accessing a resource that has intentionally been deleted. | `<all>`
    [412](#status-code-412)| Precondition Failed - returned for conditional requests, e.g. [`If-Match`](https://tools.ietf.org/html/rfc7232#section-3.1) if the condition failed. Used for optimistic locking. | `PUT`, [`PATCH`](#patch), [`DELETE`](#delete)
    [415](#status-code-415) | Unsupported Media Type - e.g. clients sends request body without content type. | [`POST`](#post), `PUT`, [`PATCH`](#patch), [`DELETE`](#delete)
    [423](#status-code-423) | Locked - Pessimistic locking, e.g. processing states. | `PUT`, [`PATCH`](#patch), [`DELETE`](#delete)
    [428](#status-code-428) | Precondition Required - server requires the request to be conditional, e.g. to make sure that the "lost update problem" is avoided (see [**MAY** consider to support `Prefer` header to handle processing preferences](#181)).|`<all>`
    [429](#status-code-429) | Too many requests - the client does not consider rate limiting and sent too many requests (see [**MUST** use code 429 with headers for rate limits](#153)).| `<all>`

   ### [Server side error codes:](#server-side-error-codes)

   Code | Meaning | Methods
    |-|-|-|
    [500](#status-code-500)| Internal Server Error - a generic error indication for an unexpected server execution problem (here, client retry may be sensible)| `<all>`
    [501](#status-code-501)|Not Implemented - server cannot fulfill the request (usually implies future availability, e.g. new feature). | `<all>`
    [503](#status-code-503) | Service Unavailable - service is (temporarily) not available (e.g. if a required component or downstream service is not available) — client retry may be sensible. If possible, the service should indicate how long the client should wait by setting the [`Retry-After`](https://tools.ietf.org/html/rfc7231#section-7.1.3) header. | `<all>`


  ## **MUST** use most specific HTTP status codes
    
    You must use the most specific HTTP status code when returning information about your request processing status or error situations.

    For example returning a 500 status error code for a resource not found (404) would be a violation of this rule

   
# HTTP Headers
  ## **MAY** use standard headers
  Use [this list](http://en.wikipedia.org/wiki/List_of_HTTP_header_fields) and explicitly mention its support in your OpenAPI definition.

  ## **SHOULD** use kebab-case with uppercase separate words for HTTP headers

  This convention is followed by (most of) the standard headers e.g. as defined in [RFC 2616](https://tools.ietf.org/html/rfc2616) and [RFC 4229](https://tools.ietf.org/html/rfc4229). Examples:

    If-Modified-Since
    Accept-Encoding
    Content-ID
    Language

  Note, HTTP standard defines headers as case-insensitive ([RFC 7230, p.22](https://tools.ietf.org/html/rfc7230#page-22)). However, for sake of readability and consistency you should follow the convention when using standard or proprietary headers. Exceptions are common abbreviations like `ID`.

  ## **MAY** consider to support Idempotency-Key header

  When creating or updating resources it can be helpful or necessary to ensure a strong idempotent behavior to prevent duplicate execution in case of retries. Generally, this can be achieved by sending a client specific _unique request key_ – that is not part of the resource  via `Idempotency-Key`header.

  The _unique request key_ is stored temporarily, e.g. for 24 hours, together with the response and the request hash (optionally) of the first request in a key cache, regardless of whether it succeeded or failed. The service can now look up the _unique request key_ in the key cache and serve the response from the key cache, instead of re-executing the request, to ensure idempotent behavior. Optionally, it can check the request hash for consistency before serving the response. If the key is not in the key store, the request is executed as usual and the response is stored in the key cache.

  This allows clients to safely retry requests after timeouts, network outages, etc. while receive the same response multiple times. 
  

  The `Idempotency-Key` header must be defined as follows, but you are free to choose your expiration time:

      components:
        headers:
        - Idempotency-Key:
            description: |
              The idempotency key is a free identifier created by the client to
              identify a request. It is used by the service to identify subsequent
              retries of the same request and ensure idempotent behavior by sending
              the same response without executing the request a second time.
      
              Clients should be careful as any subsequent requests with the same key
              may return the same response without further check. Therefore, it is
              recommended to use an UUID version 4 (random) or any other random
              string with enough entropy to avoid collisions.
      
              Idempotency keys expire after 24 hours. Clients are responsible to stay
              within this limit, if they require idempotent behavior.
      
            type: string
            format: uuid
            required: false
            example: "7da7a728-f910-11e6-942a-68f728c1ba70"

  

  ## **SHOULD** use only the specified proprietary AMRP headers (provide list)

  As a general rule, proprietary HTTP headers should be avoided. From a conceptual point of view, the business semantics and intent of an operation should always be expressed via the URLs path and query parameters, the method, and the content, but not via proprietary headers. Headers are typically used to implement protocol processing aspects, such as flow control, content negotiation, and authentication, and represent business agnostic request modifiers that provide generic context information ([RFC 7231](https://tools.ietf.org/html/rfc7231#section-5)).

  However, the exceptional usage of proprietary headers is still helpful when domain-specific generic context information…​

  1.  needs to be passed end to end along the service call chain (even if not all called services use it as input for steering service behavior and/or…​
      
  2.  is provided by specific gateway components.
      

  Below, we explicitly define the list of proprietary header exceptions usable for all services for passing through generic context information of our fashion domain (use case 1).

  Remember that HTTP header field names are not case-sensitive:


  Header field name &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | Type | Description | Header field value example
  |-|-|-|-|
  Idempotency-Key | String | Used to indicate idempotent handling of the request | 9f8b3ca3-4be5-436c-a847-9cd55460c495
  Collector-Number | String | Used to indicate the collector number | 11111111

  
  ## **MUST** propagate proprietary headers

  All LoyatyOne’s proprietary headers listed above are end-to-end headers and must be propagated to the services down the call chain. The header names and values must remain unchanged.

  
# REST Compliance Levels

  ## **MUST** use REST maturity level 2

  We strive for a good implementation of [REST Maturity Level 2](http://martinfowler.com/articles/richardsonMaturityModel.html#level2) as it enables us to build resource-oriented APIs that make full use of HTTP verbs and status codes. 
  Although this is not HATEOAS, it should not prevent you from designing proper link relationships in your APIs as stated in rules below.

  ## **MAY** use REST maturity level 3 - HATEOAS 
  We do not generally recommend implementing [REST Maturity Level 3](http://martinfowler.com/articles/richardsonMaturityModel.html#level3). HATEOAS comes with additional API complexity without real value in our SOA context where client and server interact via REST APIs and provide complex business functions as part of our e-commerce SaaS platform.

  For major concerns regarding the promised advantages of HATEOAS (see also [RESTistential Crisis over Hypermedia APIs](https://www.infoq.com/news/2014/03/rest-at-odds-with-web-apis), [Why I Hate HATEOAS](https://jeffknupp.com/blog/2014/06/03/why-i-hate-hateoas/) and others for a detailed discussion):

  *   We follow the [API First principle](#100) with APIs explicitly defined outside the code with standard specification language. HATEOAS does not really add value for SOA client engineers in terms of API self-descriptiveness: a client engineer finds necessary links and usage description (depending on resource state) in the API reference definition anyway.
        
  *   Generic HATEOAS clients which need no prior knowledge about APIs and explore API capabilities based on hypermedia information provided, is a theoretical concept that we haven’t seen working in practice and does not fit to our SOA set-up. The OpenAPI description format (and tooling based on OpenAPI) doesn’t provide sufficient support for HATEOAS either.
        
  *   In practice relevant HATEOAS approximations (e.g. following specifications like HAL or JSON API) support API navigation by abstracting from URL endpoint and HTTP method aspects via link types. So, Hypermedia does not prevent clients from required manual changes when domain model changes over time.
        
  *   Hypermedia make sense for humans, less for SOA machine clients. We would expect use cases where it may provide value more likely in the frontend and human facing service domain.
        
  *   Hypermedia does not prevent API clients to implement shortcuts and directly target resources without 'discovering' them.
    
  However, we do not forbid HATEOAS; you could use it, if you checked its limitations and still see clear value for your usage scenario that justifies its additional complexity.


# Performance

  ## **SHOULD** reduce bandwidth needs and improve responsiveness

  APIs should support techniques for reducing bandwidth based on client needs. This holds for APIs that (might) have high payloads and/or are used in high-traffic scenarios like the public Internet and telecommunication networks. Typical examples are APIs used by mobile web app clients with (often) less bandwidth connectivity. (LoyatyOne is a 'Mobile First' company, so be mindful of this point.)

  Common techniques include:

  * compression of request and response bodies using compression techniques with a strong preference for GZip
      
  * querying field filters to retrieve a subset of resource attributes (see `**SHOULD** support partial responses via filtering` below)
      
  * [ETag`](https://tools.ietf.org/html/rfc7232#section-2.3) and [`If-Match`](https://tools.ietf.org/html/rfc7232#section-3.1)/[`If-None-Match`](https://tools.ietf.org/html/rfc7232#section-3.2) headers to avoid re-fetching of unchanged resources
      
  * [Using Pagination for incremental access of larger collections of data items
      
  * caching of master data items, i.e. resources that change rarely or not at all after creation.  

  ## **SHOULD** use gzip compression

  Compress the payload of your API’s responses with gzip, unless there’s a good reason not to — for example, you are serving so many requests that the time to compress becomes a bottleneck. This helps to transport data faster over the network (fewer bytes) and makes frontends respond faster.

  Though gzip compression might be the default choice for server payload, the server should also support payload without compression and its client control via [`Accept-Encoding`](https://tools.ietf.org/html/rfc7231#section-5.3.4) request header — see also [RFC 7231 Section 5.3.4](https://tools.ietf.org/html/rfc7231#section-5.3.4). The server should indicate used gzip compression via the [`Content-Encoding`](https://tools.ietf.org/html/rfc7231#section-3.1.2.2) header.


  ## **SHOULD** support partial responses via filtering

  Compress the payload of your API’s responses with gzip, unless there’s a good reason not to — for example, you are serving so many requests that the time to compress becomes a bottleneck. This helps to transport data faster over the network (fewer bytes) and makes frontends respond faster.

Though gzip compression might be the default choice for server payload, the server should also support payload without compression and its client control via [`Accept-Encoding`](https://tools.ietf.org/html/rfc7231#section-5.3.4) request header — see also [RFC 7231 Section 5.3.4](https://tools.ietf.org/html/rfc7231#section-5.3.4). The server should indicate used gzip compression via the [`Content-Encoding`](https://tools.ietf.org/html/rfc7231#section-3.1.2.2) header.

## **SHOULD** support partial responses via filtering

  Depending on your use case and payload size, you can significantly reduce network bandwidth need by supporting filtering of returned entity fields. Here, the client can explicitly determine the subset of fields he wants to receive via the [`fields`](#fields) query parameter. (It is analogue to [GraphQL `fields`](https://graphql.org/learn/queries/#fields) and simple queries, and also applied, for instance, for [Google Cloud API’s partial responses](https://cloud.google.com/storage/docs/json_api/v1/how-tos/performance#partial-response).)

  ### Unfiltered

      GET http://api.example.org/users/123 HTTP/1.1
      
      HTTP/1.1 200 OK
      Content-Type: application/json
      
      {
        "id": "cddd5e44-dae0-11e5-8c01-63ed66ab2da5",
        "name": "John Doe",
        "address": "1600 Pennsylvania Avenue Northwest, Washington, DC, United States",
        "birthday": "1984-09-13",
        "friends": [ {
          "id": "1fb43648-dae1-11e5-aa01-1fbc3abb1cd0",
          "name": "Jane Doe",
          "address": "1600 Pennsylvania Avenue Northwest, Washington, DC, United States",
          "birthday": "1988-04-07"
        } ]
      }

  ### Filtered

      GET http://api.example.org/users/123?fields=(name,friends(name)) HTTP/1.1
      
      HTTP/1.1 200 OK
      Content-Type: application/json
      
      {
        "name": "John Doe",
        "friends": [ {
          "name": "Jane Doe"
        } ]
      }

  The `fields` query parameter determines the fields returned with the response payload object. For instance, `(name)` returns `users` root object with only the `name` field, and `(name,friends(name))` returns the `name` and the nested `friends` object with only its `name` field.

  OpenAPI doesn’t support you in formally specifying different return object schemes depending on a parameter. When you define the field parameter, we recommend to provide the following description: `Endpoint supports filtering of return object fields as described in [Rule #157]([https://opensource.loyaltyone.com/restful-api-guidelines/#157](https://opensource.loyaltyone.com/restful-api-guidelines/#157))`

  The syntax of the query `fields` value is defined by the following [BNF](https://en.wikipedia.org/wiki/Backus%E2%80%93Naur_form) grammar.

      <fields>            ::= [ <negation> ] <fields_struct>
      <fields_struct>     ::= "(" <field_items> ")"
      <field_items>       ::= <field> [ "," <field_items> ]
      <field>             ::= <field_name> | <fields_substruct>
      <fields_substruct>  ::= <field_name> <fields_struct>
      <field_name>        ::= <dash_letter_digit> [ <field_name> ]
      <dash_letter_digit> ::= <dash> | <letter> | <digit>
      <dash>              ::= "-" | "_"
      <letter>            ::= "A" | ... | "Z" | "a" | ... | "z"
      <digit>             ::= "0" | ... | "9"
      <negation>          ::= "!"

  **Note:** Following the [principle of least astonishment](https://en.wikipedia.org/wiki/Principle_of_least_astonishment), you should not define the `fields` query parameter using a default value, as the result is counter-intuitive and very likely not anticipated by the consumer.



# Compatibility and Versioning
 ## **MUST** not break backward compatibility 

Change APIs, but keep all consumers running. Consumers usually have independent release lifecycles, focus on stability, and avoid changes that do not provide additional value. APIs are contracts between service providers and service consumers that cannot be broken via unilateral decisions.

There are two techniques to change APIs without breaking them:

*   follow rules for compatible extensions
    
*   introduce new API versions and still support older versions with `deprecation`
    

We strongly encourage using compatible API extensions and discourage versioning (see `**SHOULD** avoid versioning` and `**MUST** use media type versioning` below). The following guidelines for service providers (`**SHOULD** prefer compatible extensions`) and consumers (`**MUST** prepare clients to accept compatible API extensions`) enable us (having Postel’s Law in mind) to make compatible changes without versioning.

**Note:** There is a difference between incompatible and breaking changes. Incompatible changes are changes that are not covered by the compatibility rules below. Breaking changes are incompatible changes deployed into operation, and thereby breaking running API consumers. Usually, incompatible changes are breaking changes when deployed into operation. However, in specific controlled situations it is possible to deploy incompatible changes in a non-breaking way, if no API consumer is using the affected API aspects (see also `Deprecation` guidelines).


## **SHOULD** prefer compatible extensions

  API designers should apply the following rules to evolve RESTful APIs for services in a backward-compatible way:

  * Add only optional, never mandatory fields.
      
  * Never change the semantic of fields (e.g. changing the semantic from collector-number to collector-id, as both are different unique customer keys)
      
  * Input fields may have (complex) constraints being validated via server-side business logic. Never change the validation logic to be more restrictive and make sure that all constraints are clearly defined in description.
      
  * Enum ranges can be reduced when used as input parameters, only if the server is ready to accept and handle old range values too. 
      
  * Enum ranges cannot be extended when used for output parameters — clients may not be prepared to handle it. However, enum ranges can be extended when used for input parameters.
      
  * Support redirection in case an URL has to change `301 (Moved Permanently)`.


## **SHOULD** design APIs conservatively

Designers of service provider APIs should be conservative and accurate in what they accept from clients:

*   Unknown input fields in payload or URL should not be ignored; servers should provide error feedback to clients via an HTTP 400 response code.
    
*   Be accurate in defining input data constraints (like formats, ranges, lengths etc.) — and check constraints and return dedicated error information in case of violations.
    
*   Prefer being more specific and restrictive (if compliant to functional requirements), e.g. by defining length range of strings. It may simplify implementation while providing freedom for further evolution as compatible extensions.
    

Not ignoring unknown input fields is a specific deviation from Postel’s Law (e.g. see also [The Robustness Principle Reconsidered](https://cacm.acm.org/magazines/2011/8/114933-the-robustness-principle-reconsidered/fulltext)). Servers might want to take different approach but should be aware of the following problems and be explicit in what is supported:

*   Ignoring unknown input fields is actually not an option for `PUT`, since it becomes asymmetric with subsequent `GET` response and HTTP is clear about the `PUT` _replace_ semantics and default roundtrip expectations (see [RFC 7231 Section 4.3.4](https://tools.ietf.org/html/rfc7231#section-4.3.4)). Note, accepting (i.e. not ignoring) unknown input fields and returning it in subsequent `GET` responses is a different situation and compliant to `PUT` semantics.
    
*   Certain client errors cannot be recognized by servers, e.g. attribute name typing errors will be ignored without server error feedback. The server cannot differentiate between the client intentionally providing an additional field versus the client sending a mistakenly named field, when the client’s actual intent was to provide an optional input field.
    
*   Future extensions of the input data structure might be in conflict with already ignored fields and, hence, will not be compatible, i.e. break clients that already use this field but with different type.
    

In specific situations, where a (known) input field is not needed anymore, it either can stay in the API definition with "not used anymore" description or can be removed from the API definition as long as the server ignores this specific parameter.

## **MUST** prepare clients to accept compatible API extensions

Service clients should apply the robustness principle:
   
*   Be tolerant in processing and reading data of API responses, more specifically service clients must be prepared for compatible API extensions of response data:
    
    *   Be tolerant with unknown fields in the payload (see also Fowler’s ["TolerantReader"](http://martinfowler.com/bliki/TolerantReader.html) post), i.e. ignore new fields but do not eliminate them from payload if needed for subsequent `PUT` requests.
        
    *   Be prepared that `Enum` return parameter may deliver new values; either be agnostic or provide default behavior for unknown values.
        
    *   Be prepared to handle HTTP status codes not explicitly specified in endpoint definitions. Note also, that status codes are extensible. Default handling is how you would treat the corresponding `2xx` code (see [RFC 7231 Section 6](https://tools.ietf.org/html/rfc7231#section-6)).
        
    *   Follow the redirect when the server returns HTTP status code `301 (Moved Permanently)`.
        
    

## **MUST** treat OpenAPI specification as open for extension by default

The OpenAPI specification is not very specific on default extensibility of objects, and redefines JSON-Schema keywords related to extensibility, like `additionalProperties`. Following our compatibility guidelines, OpenAPI object definitions are considered open for extension by default as per [Section 5.18 "additionalProperties"](http://json-schema.org/latest/json-schema-validation.html#rfc.section.5.18) of JSON-Schema.

When it comes to OpenAPI, this means an `additionalProperties` declaration is not required to make an object definition extensible:

* API clients consuming data must not assume that objects are closed for extension in the absence of an `additionalProperties` declaration and must ignore fields sent by the server they cannot process. This allows API servers to evolve their data formats.
    
* For API servers receiving unexpected data, the situation is slightly different. Instead of ignoring fields, servers _may_ reject requests whose entities contain undefined fields in order to signal to clients that those fields would not be stored on behalf of the client. API designers must document clearly how unexpected fields are handled for `PUT`, [`POST`](#post), and [`PATCH`](#patch) requests.
  
API formats must not declare `additionalProperties` to be false, as this prevents objects being extended in the future.

## **SHOULD** avoid versioning

When changing your RESTful APIs, do so in a compatible way and avoid generating additional API versions. Multiple versions can significantly complicate understanding, testing, maintaining, evolving, operating and releasing our systems ([supplementary reading](http://martinfowler.com/articles/enterpriseREST.html)).

If changing an API can’t be done in a compatible way, then proceed in one of these three ways:

*   create a new resource (variant) in addition to the old resource variant
    
*   create a new service endpoint — i.e. a new application with a new API (with a new domain name)
    
*   create a new API version supported in parallel with the old API by the same microservice
    
As we discourage versioning by all means because of the manifold disadvantages, we strongly recommend to only use the first two approaches.

## [**MUST** not use URL versioning

With URL versioning a (major) version number is included in the path, e.g. `/v1/customers`. The consumer has to wait until the provider has been released and deployed. If the consumer also supports hypermedia links — even in their APIs — to drive workflows (HATEOAS), this quickly becomes complex. So does coordinating version upgrades — especially with hyperlinked service dependencies — when using URL versioning. To avoid this tighter coupling and complexer release management we do not use URL versioning


# Data formats
-----------------------------------------------

### [**SHOULD** use standard data formats \[238\]](#238)

[Open API](https://github.com/OAI/OpenAPI-Specification/blob/main/versions/3.1.0.md#data-types) (based on [JSON Schema Validation vocabulary](https://tools.ietf.org/html/draft-bhutton-json-schema-validation-00#section-7.3)) defines formats from ISO and IETF standards for date/time, integers/numbers and binary data. You **must** use these formats, whenever applicable:

   

`OpenAPI type` | `OpenAPI format` |  Specification | Example |
-|-|-|-|
`integer` | `int32` | 4 byte signed integer between -231 and 231\-1 | `7721071004`
`integer` | `int64` | 8 byte signed integer between -263 and 263\-1 | `772107100456824`
`integer` | `bigint` | arbitrarily large signed integer number |  `77210710045682438959`
`number` |  `float` | `binary32` single precision decimal number — see [IEEE 754-2008/ISO 60559:2011](https://en.wikipedia.org/wiki/IEEE_754) | `3.1415927`
`number` | `double` |`binary64` double precision decimal number — see [IEEE 754-2008/ISO 60559:2011](https://en.wikipedia.org/wiki/IEEE_754) | `3.141592653589793`
`number` |  `decimal` | arbitrarily precise signed decimal number | `3.141592653589793238462643383279`
`string` | `byte` | `base64url` encoded byte following [RFC 7493 Section 4.4](https://tools.ietf.org/html/rfc7493#section-4.4) | `"VA=="`
`string` | `binary` | `base64url` encoded byte sequence following [RFC 7493 Section 4.4](https://tools.ietf.org/html/rfc7493#section-4.4) | `"VGVzdA=="`
`string` | `date` | [RFC 3339](https://tools.ietf.org/html/rfc3339) internet profile — subset of [ISO 8601](https://tools.ietf.org/html/rfc3339#ref-ISO8601) | `"2019-07-30"`
`string` | `date-time` | [RFC 3339](https://tools.ietf.org/html/rfc3339) internet profile — subset of [ISO 8601](https://tools.ietf.org/html/rfc3339#ref-ISO8601) | `"2019-07-30T06:43:40.252Z"`
`string` | `time` | [RFC 3339](https://tools.ietf.org/html/rfc3339) internet profile — subset of [ISO 8601](https://tools.ietf.org/html/rfc3339#ref-ISO8601) | `"06:43:40.252Z"`
`string` |  `duration` | [RFC 3339](https://tools.ietf.org/html/rfc3339) internet profile — subset of [ISO 8601](https://tools.ietf.org/html/rfc3339#ref-ISO8601) | `"P1DT30H4S"`
`string` | `period` | [RFC 3339](https://tools.ietf.org/html/rfc3339) internet profile — subset of [ISO 8601](https://tools.ietf.org/html/rfc3339#ref-ISO8601) | `"2019-07-30T06:43:40.252Z/PT3H"`
`string` | `password` |  | `"secret"` 
`string` | `email` | [RFC 5322](https://tools.ietf.org/html/rfc5322) | `"[example@loyaltyone.de](mailto:example@loyaltyone.de)"`
`string` | `idn-email` | [RFC 6531](https://tools.ietf.org/html/rfc6531)| `"hello@bücher.example"`
`string` | `hostname` | [RFC 1034](https://tools.ietf.org/html/rfc1034) | `"www.loyaltyone.de"`
`string`| `idn-hostname`| [RFC 5890](https://tools.ietf.org/html/rfc5890)|`"bücher.example"`
`string` | `ipv4`| [RFC 2673](https://tools.ietf.org/html/rfc2673) | `"104.75.173.179"`
`string` | `ipv6` | [RFC 2673](https://tools.ietf.org/html/rfc2673) | `"2600:1401:2::8a"`
`string` | `uri` | [RFC 3986](https://tools.ietf.org/html/rfc3986) | `"https://www.loyaltyone.de/"`
`string` | `uri-reference` | [RFC 3986](https://tools.ietf.org/html/rfc3986) | `"/clothing/"`
`string` | `uri-template` | [RFC 6570](https://tools.ietf.org/html/rfc6570)| `"/users/{id}"`
`string` | `iri` | [RFC 3987](https://tools.ietf.org/html/rfc3987) | `"https://bücher.example/"`
`string` | `iri-reference`| [RFC 3987](https://tools.ietf.org/html/rfc3987) | `"/damenbekleidung-jacken-mäntel/"`
`string` | [`uuid`](#144) | [RFC 4122](https://tools.ietf.org/html/rfc4122) | `"e2ab873e-b295-11e9-9c02-…​"`
`string` | `json-pointer` | [RFC 6901](https://tools.ietf.org/html/rfc6901) | `"/items/0/id"`
`string` | `relative-json-pointer` | [Relative JSON pointers](https://tools.ietf.org/html/draft-handrews-relative-json-pointer) | `"1/id"`


We add further OpenAPI formats based other ISO and IETF standards. You **must** use these formats, whenever applicable:

`OpenAPI type` | `format` | Specification | Example
|-|-|-|-|
`string`| `iso-639` | two letter language code — see [ISO 639-1](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes)|`"en"`
`string` | `bcp47` | multi letter language tag — see [BCP 47](https://tools.ietf.org/html/bcp47). It is a compatible extension of [ISO 639-1](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes) optionally with additional information for language usage, like region, variant, script. | `"en-DE"`
`string` | `iso-3166` | two letter country code — see [ISO 3166-1 alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) | `"GB"` 
`string` | `iso-4217` | three letter currency code — see [ISO 4217](https://en.wikipedia.org/wiki/ISO_4217) | `"EUR"` 
`string` | `gtin-13` | Global Trade Item Number — see [GTIN](https://en.wikipedia.org/wiki/Global_Trade_Item_Number) | `"5710798389878"`
`string` | `regex` | regular expressions as defined in [ECMA 262](http://www.ecma-international.org/publications/files/ECMA-ST/Ecma-262.pdf) | `"^[a-z0-9]+$"`

**Remark:** Please note that this list of standard data formats is not exhaustive and everyone is encouraged to propose additions.

## [**MUST** use standard formats for date and time properties \[169\]](#169)

As a specific case of `**SHOULD** use standard data formats`, you must use the `string` typed formats `date`, `date-time`, `time`, `duration`, or `period` for the definition of date and time properties. The formats are based on the standard [RFC 3339](https://tools.ietf.org/html/rfc3339) internet profile -- a subset of [ISO 8601](https://tools.ietf.org/html/rfc3339#ref-ISO8601)

**Exception:** For passing date/time information via standard protocol headers, HTTP [RFC 7231](https://tools.ietf.org/html/rfc7231#section-7.1.1.1) requires to follow the date and time specification used by the Internet Message Format [RFC 5322](https://tools.ietf.org/html/rfc5322).

As defined by the standard, time zone offset may be used, however, we recommend to only use times based on UTC without local offsets. For example `2015-05-28T14:07:17Z` rather than `2015-05-28T14:07:17+00:00`. From experience we have learned that zone offsets are not easy to understand and often not correctly handled. Note also that zone offsets are different from local times which may include daylight saving time. When it comes to storage, all dates should be consistently stored in UTC without a zone offset. Localization should be done locally by the services that provide user interfaces, if required.


## [**MUST** use standard formats for country, language and currency properties \[170\]](#170)

As a specific case of `**MUST** use standard data formats` you must use the following standard formats:

*   Country codes: [ISO 3166-1-alpha2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) two letter country codes indicated via OpenAPI format `iso-3166`
    
*   Language codes: [ISO 639-1](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes) two letter language codes indicated via OpenAPI format `iso-639`
    
*   Language variant tags: [BCP 47](https://tools.ietf.org/html/bcp47) multi letter language tag indicated via OpenAPI format `bcp47`. (It is a compatible extension of [ISO 639-1](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes) with additional optional information for language usage, like region, variant, script)
    
*   Currency codes: [ISO 4217](https://en.wikipedia.org/wiki/ISO_4217) three letter currency codes indicated via OpenAPI format `iso-4217`
    

## **SHOULD** use content negotiation, if clients may choose from different resource representations

In some situations the API supports serving different representations of a specific resource (at the same URL) e.g. JSON, PDF, TEXT, or HTML representations for an invoice resource. You should use [content negotiation](https://en.wikipedia.org/wiki/Content_negotiation) to support clients specifying via the standard HTTP headers [`Accept`](https://tools.ietf.org/html/rfc7231#section-5.3.2), [`Accept-Language`](https://tools.ietf.org/html/rfc7231#section-5.3.5), [`Accept-Encoding`](https://tools.ietf.org/html/rfc7231#section-5.3.4) which representation is best suited for their use case, for example, which language of a document, representation / content format, or content encoding. You [**SHOULD** use standard media types](#172) like `application/json` or `application/pdf` for defining the content format in the [`Accept`](https://tools.ietf.org/html/rfc7231#section-5.3.2) header.


# References

https://www.ics.uci.edu/~fielding/pubs/dissertation

https://docs.microsoft.com/en-us/azure/architecture/best-practices/api-design

https://cloud.google.com/apis/design

https://opensource.zalando.com/restful-api-guidelines

https://martinfowler.com/articles/richardsonMaturityModel.html

