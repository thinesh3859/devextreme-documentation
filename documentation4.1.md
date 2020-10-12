# Financial Data Exchange (FDX)<br/>API and Data Structures Working Group<br/>Version 4.1<br/>April, 2020

**FS-ISAC Traffic Light Protocol (TLP) Green:** Recipients may share TLP GREEN information with peers, trusted government and critical infrastructure partner organizations, and service providers with whom they have a contractual relationship, who have a need-to-know but not via publicly accessible channels.


# Change Log

| Version | Date           | Originator    | Reason for Change                                            | Ratified |
|---------|----------------|---------------|--------------------------------------------------------------|----------|
| 1.0     | May 2015       | Anoop Saxena  | Initial Document                                             | Yes      |
| 2.0     | December 2017  | Anil Mahalaha | Entities Updated: <br/> * 12.7 Capability Entity -- new fields <br/> * 12.12 Debt Security - new fields like call type <br/> * 13.19  Holding - one change <br/> * 12.27 LOC Account | Yes  |
|         |                |               | Entities Added: <br/> *12.49-12.57 Tax Form Data <br/> *12.58 Insurance Account <br/> *12.59 Insurance Transaction <br/> *12.60 Bill <br/> *12.61 Pension Source <br/> *12.62 Annuity Account | |
|         |                |               | Simple Types Added: <br/> *13.53 Annuity Product <br/> *13.54 Annuity Value Basis <br/> *13.55 Annual Increase Type <br/> *13.56 Period Certain Guarantee                           | |
| 2.1     | September 2018 | Anil Mahalaha | Updated Section 8 - Logical Data Model                       | Yes      |
|         |                |               | Section 7: Removed security guidelines in favor of FS-ISAC Aggregation Services Working Group's document "Control Considerations for Consumer Financial Account Aggregation Services - Reference Security Architecture and Standard Specification" Version 2.1 -June 2018 for securing FDX API.                                            |          |
|         |                |               | Section 7: Add Scopes (Tax and Insurance)                    |          |
|         |                |               | Section 8: Changed to allow Max 256 Char                     |          |
|         |                |               | Section 10: Changed Error 501 to return HTTP Status Code 500 |          |
|         |                |               | Section 11: Added Cursor based Pagination                    |          |
|         |                |               | Section 12: Removed sample Request/Response in favor of accompanying OpenAPI Specification schema document ddav2.1_oas3.yaml <br/> * Added GET methods to all POST only resources <br/> * Changed Resource names to spinal-case <br/> * Added Resource /account/tax                                  |          |
|         |                |               | Section 13: Added Versioning                                 |          |
|         |                |               | Section 14: <br/> * Account Entity - Added accountNumberDisplay field <br/> * Accounts Entity - Added AnnuityAccount & InsuranceAccount <br/> * InsuranceAccount Entity - Added Array of Insurance Transactions <br/> * Investment Account Entity - Added Array of PensionSource <br/> * Added Tax Entity <br/> * Removed all references to XML                                |          |
| 3.0     | May 2019       | Anil Mahalaha |                                                              | Yes       |
| 4.0     | September 2019 | Ravneet Singh | Major restructuring changes as outlined in the REST Best Practices RFC and OpenAPI and PDF Inconsistencies RFC.<br/><br/>The API includes updates to comply with **_REST best practices_** and is now fully REST compliant. This is largely a technical structural change to the specification that makes for easier and more consistent implementations. Resources were relabeled to comply with REST conventions and HTTP methods are now aligned with RESTful recommendations. Different content types are served from the same endpoint. **_HATEOAS_** type links are used in tax document searches, pagination links, and transaction image results. The **_GET_** query parameter is used to determine how many fields to return.| Yes      |
|         |                | Clyde Cutting<br/>Bruce Wilcox| Added **complex types** for tax and tax entities as described in the FDX 4.0 Tax Entities and FDX 4.0 Tax APIs RFCs.<br/><br/>There has been a complete restructuring of how tax is handled within the specification. It is a breaking change, but necessary to resolve issues and to support the exchange of tax forms and form data between tax data suppliers and tax data aggregators.<br/><br/>There is a complete replacement of the Tax Entities for FDX version 4.0. These entities include significant breaking changes to the Tax Entities previously published in FDX version 3.0 documentation. This change was necessary as those entities had significant issues that prevented use by tax software and tax preparation companies, such as missing data elements, incorrectly typed data elements, not extending the base Tax entity, etc. 58 new entities have been introduced, including 34 entities corresponding to various types of tax forms.<br/><br/>New Tax APIs have been added to version 4.0 that replace the previously published single Tax API. These APIs are an entirely new suite of resource endpoints to support the exchange of tax forms and tax form data between tax data suppliers and tax data aggregators.<br/>|          |
|         |                | Anil Mahalaha |BOND has been added as a **_securityType_**.<br/>OTHER has been added as a **_holdingType_**.<br/>Updates to telephone number, address, and name handling as described in the following RFCs:<ul><li>TelephoneNumber RFC</li><li>DeliveryAddress RFC</li><li>CustomerName RFC</li></ul>The length of **_TelephoneNumber_** entity has been increased from 10 digits to 15 digits to support international subscriber telephone numbers which can have lengths greater than 10, per ITU-T recommendation E.164.<br/><br/>A new **_base address entity_** is included in API 4.0. This simplifies increased the consistency of any part of the specification where an address is required. For example, **_DeliveryAddress_** entity now extends and inherits all fields from address instead of merely duplicating some of the elements as previous.<br/><br/>A new **_IndividualName_** entity has now been added to the specification to simplify and provide consistency wherever 'human' names are required in the spec. For example, **_CustomerName_** is now modified to use the new **_IndividualName_** entity instead of defining each name element separately.<br/><br/>|          |
|         |                |Jonathan Kassan|Update of Accounts and AccountDescriptor entities as described in Account Descriptor RFC (formerly Combining Account Entity & AccountDescriptor). The AccountDescriptor entity was missing currency, which was added. The displayName field on the AccountDescriptor and InsuranceAccount entities referenced a confusing concatenation ("Account identity to display to customer. This may be a masked account number or product name followed by masked number) which is now separated into two fields: productName and accountNumberDisplay.<br/><br/>**_AccountDescriptors_** have been modified to better distinguish the fields used to describe an account. New elements **_productName_** , **_nickName_** , and **_accountNumberDisplay_** have been added to provide an easier, more consistent way to assist the end user in account selection. <ul><li>**_nickName_** is the user specified name for the account, used in user interfaces (UIs) to assist in account selection</li><li>**_productName_** is the Marketed product name for this account, used in UIs to assist in account selection</li><li>**_accountNumberDisplay_** is typically the masked account number that is displayed on a bank's UI.</li></ul>|          |
|         | February 2020  |Rich Dudley    |**Errata**|          |
|         |                |               |OPTIONS (plural) has been changed to OPTION (singular) in **_holdingType_**.|          |
|         |                |               |MILITARYLOAN and INSTITUTIONALTRUST have been fixed in **_AccountType_**.|          |
|         |                |               |Indentation corrections were made for occurrences of "Accounts" and "AccountDetailsRequest" in the fdxapi4 YAML file.|          |
|         |                |               |API Data Structures Documentation edits:<ul><li>"Th" has been corrected to "The" in section 6 Message Syntax".</li><li>Account**s** DetailsRequest" (plural) has been changed to "AccountDetailsRequest" (singular).</li></ul> |          |
| 4.1     | April 2020     | Paul Allen<br/>Clyde Cutting<br/>Bruce Wilcox | Implemented the final IRS 2019 tax form changes for **Tax1065K1**, **Tax1120SK1** |         |
|         |                |               | Corrected the Tax1099Oid field from "description" to "oidDescription" |         |
|         |                |               | Updated the tax form entity descriptions to match the YAML file |         |

# 1. Introduction

The Financial Data Exchange API and Data Structures (FDX API) enable Data Aggregators and Data Intermediaries to
obtain consumer's financial data from a financial platform provider. FDX API is intended to replace screen scraping
as a preferred method for Data Aggregators and Data Intermediaries to obtain this data. Screen scraping is not
secure as it requires consumers to share their financial platform login credential with Data Aggregators and Data
Intermediaries. FDX API uses OAuth tokens which a consumer generates on the financial platform and provides to the
Data Aggregators and Data Intermediaries. The tokens are then passed via FDX API to gain access to the consumer's
financial data.

![](media/ffd86ce80b8500f2fcc4b1ca1ba32b05.png)

![](media/6aafbe5e4765ad07be94e31170c959e5.png)

## 1.1 Definitions

### 1.1.1 Consumer

* An individual or an agent, trustee, or representative acting on behalf of an individual with ownership rights to the data and transactional capabilities at a financial platform provider.

* The individual with ownership rights to the data. This can also mean parties the consumer delegates access to like: Spouse, Bookkeeper, CPA, Attorney, Investment Advisor

### 1.1.2 Data Access Point

* Consumer financial product and service providers that rely, at least in part, on consumer-permissioned access to consumer financial account data to display data from one or more sources to a consumer and in some instances facilitate transactional capabilities on behalf of the consumer.

* The presentation layer for the data to the Consumer. This can be an application, web page, or other medium.

### 1.1.3 Data Aggregator / Data Intermediary

* Entities that obtain consumer financial data directly from consumer financial platform providers for one or more data access points.

### 1.1.4 Financial Platform Provider

* Financial Institution's System of Record (SOR) for Consumer financial data.  This can be the FI itself or a Service Bureau or other party that maintains the SOR on behalf of the FI.

# 2. Ability Criteria
This section is now deprecated and will be relocated, TBD.

<!-- ## 2.1 Performance

### 2.1.1  High throughput for large data sets and high concurrency

## 2.2 Scalability

### 2.2.1  High throughput for batch transfers and large number of concurrent users

### 2.2.2  Large transaction data sets

## 2.3 Interoperability

### 2.3.1  Self-identifying data messages (transport neutral)

### 2.3.2  Multi-version support

## 2.4 Extensibility

### 2.4.1  Data Model is extensible with the use of FiAttributes

## 2.5 Security

### 2.5.1  Consent obtained from data owners before transfer

### 2.5.2  Different data types have different scope of consent

## 2.6 Reliability

### 2.6.1  Bandwidth limiting each data partner to prevent denial-of-service (DOS) -->

# 3. Deployment

In legacy OFX, a multitude of desktop PFM applications connect to a multitude of OFX servers. In FDX API, this is replaced with a service bus model.

Data Aggregators and Data Intermediaries will consolidate requests from all application clients into a single FDX API client. The Financial platform provider's API server will authenticate and direct the incoming requests to the appropriate backend server.

![](media/d77be9715deb8e062fafa01d0f34814c.png)

# 4. Message Transport

The FDX API requires the use of HTTPS, which is a reliable synchronous
stateless message protocol. REST is preferred because it decouples the message
syntax from the transport concerns. REST supports content type negotiation,
conditional fetches, and compression. Since confidential information is being
exchanged, all interactions must be encrypted with TLS.

# 5. Service Delivery Expectations

The FDX API server response to requests must start within 30 seconds.
The server may use HTTP 100 continue or 200 chunked encoding response to extend
the response time for large data sets. Server responses should not last longer
than 120 seconds to prevent long running transactions.

# 6. Message Syntax

The FDX API only supports JSON syntax options, with the exception of image file responses.

# 7. Security

Please refer to the Financial Data Exchange Aggregation Services Working Group's document "Control Considerations for Consumer Financial Account Aggregation Services - Reference Security Architecture and Standard Specification" Version 2.1 -- June 2018 for securing FDX API.

Sections "Component Model: Guidelines for Authentication and Aggregation Data Feeds" and "Deployment Model: Implementation Specifications / Technology" are of special interest as you look secure FDX APIs.

## 7.1 Token Scope

The FDX API client application will include a list of desired scopes when requesting an authorization token. The following scopes are defined for FDX API data service.

| **Primary Entity** | **Allowed Actions**                                          | **Token Scope**      |
|--------------------|--------------------------------------------------------------|----------------------|
| Account            | Read only Access to summary account information              | FinancialInformation |
| Customer           | Read only Access to customer information, including PII      | FinancialInformation |
| Image              | Read only Access to transaction images (checks and receipts) | FinancialInformation |
| Statement          | Read only Access to statement image                          | FinancialInformation |
| Transfer           | Transfer of money between accounts                           | Transfer             |
| Transaction        | Read only Access to transaction information                  | FinancialInformation |
| Tax                | Read only Access to tax information                          | FinancialInformation |
| Insurance          | Read only Access to Insurance Information                    | FinancialInformation |

The FDX API server will return the list of allowed scopes with the issued authorization token. The FDX API server may limit the scopes for the purpose of not implementing certain APIs.

The FDX API server may also present scopes in the access confirmation page after end user login to have them determine each account(s) access for the requesting application.

# 8. Logical Data Model

FDX API will eventually encompass multiple financial data domains. At this point, entities and messages are required to support the aggregation of personal financial data.

![https://raw.githubusercontent.com/FSISAC-Data-Aggregation-Forum/dda/master/images/entity%20model.png?token=AK0DXIByaiZhAFqYobg8UCT5LXYamT9Pks5bixSSwA%3D%3D](media/9a5436eca2f135074d94e3b2a55f75c8.png)

## 8.1 Entity Identity

The User entity is not expressed in FDX API messages. The Login entity
has an identity unique to its owning Institution. The Login identity is usually
the username part of a username / password login. The Login surrogate identity
is the OAuth token obtained from the Financial Institution. The Account entity
has an identity that is unique to the owning Institution. The Transaction entity
has an identity that is unique to the owning Account and is usually unique to
the owning Institution.

The entity identity (or surrogate identity) is required when transmitting the
entities and is used to relate the entities. The FDX API identity
properties have a maximum of 256 characters. (IBAN account identifiers are 31
characters, ACH has 9 digits for routing,17 digits for account number, and
SHA-256 generates an almost-unique 256-bit (32-byte) signature for a given
text.)

## 8.2 Surrogate Identity

OAuth creates a surrogate identity for a Login -- a FDX API server
does not expose the financial institution's principal identity of the  Login.
To limit the exposure of personally identifiable information, the other
identities transmitted by the FDX API server should be a surrogate
identity. Surrogates must provide the same uniqueness constraints on the
entity relationships as described above. Any surrogate identities must be
long term persistent.

If the Financial platform provider's account identity is considered
confidential, a surrogate identity should be used (AccountId should not equal
AccountNumber).

# 9. Residual Data

Residual data is defined as data that is no longer being used, for example if an
account has been closed. Data Aggregators should delete residual data from their
systems within 180 days.

# 10. Protocol

The FDX API client requests data using HTTP [GET](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html#sec9.3), [POST](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html#sec9.5) and [PUT](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html#sec9.6)) methods. The request includes an appropriate [Request-URI](http://www.w3.org/Protocols/rfc2616/rfc2616-sec5.html#sec5.1.2). Requests must include an OAuth token in the authorization header. The following is a sample of the headers provided in a typical request.

```http
GET /accounts HTTP/1.1
Host: example.com
Authorization: Bearer w0mcJylzCn-AfvuGdqkty2-KP48=
Accept: application/json
Accept-Charset: UTF-8 Accept-Encoding: gzip
```

The FDX API server will use [HTTP status
codes](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) to indicate the
success or failure of a request. Response code details specific to FDX
API follow. For status codes other than 200, the HTTP response body must contain
an Error Entity.

## 10.1 Headers

### 10.1.1 Transport Security

All FDX API communication must be secured from network sniffing with
SSL/TLS. Using TLS will secure the entire request and response including any
headers. We recommend that both the FDX API client and server use
certificates. Additionally, FDX API server responses should include
[Cache-Control](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
headers

```http
Cache-Control: no-cache, no-store
```

to prevent any caching or storing of the response.

### 10.1.2 Request Authorization

The FDX API client does not identify a User to the FDX API
server. Instead, the User's financial institution Login is implied via an
[OAuth](https://tools.ietf.org/html/rfc6749) token. The data returned by any
FDX API request is limited to what the User could see using his/her
Login and further limited by the scope of the
OAuth token.

The FDX API client uses the
[Authorization](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.8)
request header with a [Bearer](http://tools.ietf.org/html/rfc6750) or
[MAC](http://tools.ietf.org/html/draft-ietf-oauth-v2-http-mac-01) token. Bearer
tokens are recommended although the server has option to issue MAC tokens as an
alternative if the client supports it. How to obtain this token was detailed in
the Security Model section.

```http
Authorization: Bearer w0mcJylzCn-AfvuGdqkty2-KP48=
```

### 10.1.3 Content Negotiation

The FDX API clients and servers use standard HTTP headers to negotiate
transport options.

The FDX API client uses the
[Accept](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.1) request
header to ask for its preferred syntax. The server must respond with one of the
requested syntaxes or with a 406 status code.

```http
Accept: application/json
```

The FDX API client uses the
[Accept-Charset](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.2)
request header to ask for its preferred character set. The server must respond
with the body encoded in one of the requested character sets or with a 406
status code.

```http
Accept-Charset: UTF-8
```

The FDX API server uses the
[Content-Type](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.17)
response header to inform the client of the response syntax and charset.

```http
Content-Type: application/json; charset=UTF-8
```

The FDX API client uses the
[Accept-Encoding](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.3)
request header to ask for its preferred compression encoding. The server must
either respond with the body compressed with one of the requested compressions,
or with the body not compressed.

```http
Accept-Encoding: compress, gzip
```

The FDX API server uses the
[Content-Encoding](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.11)
response header to inform the client of the response encoding.

```http
Content-Encoding: gzip
```

For queries, the FDX API client may use the
[If-Modified-Since](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.25)
request header to ask for a data response only if the data has been modified
since the given date. If the server supports this header and the data has not
been modified, a 304 HTTP response code will be returned to the client.

```http
If-Modified-Since: Wed, 12 Sep 2012 06:00:00 GMT
```

### 10.1.4 Server Environment

The FDX API server returns a
[Date](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.18) header
with every response.

```http
Date: Tue, 11 Sep 2012 19:43:31 GMT
```

### 10.1.5 Host

The [Host](http://www.w3.org/Protocols/rfc2616/rfc2616-sec3.html#sec3.2.2)
request header field specifies the Internet host and port number of the resource
being requested. A
Host header
without any trailing port information implies the default port for the service
requested (e.g. "80" for an HTTP URL).

```http
Host: example.com
```

### 10.1.6 Client Identity

The FDX API client supplies a
[User-Agent](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.43)
header with every request. This header should not be used to change the content
of the response. This header is designed to only collect statistics on the
products using the FDX API data service. The first token is the Data
Aggregator and Data AccessPoint version. The second token is the product and
product version.

```http
User-Agent: Example/1.2.3 Crawler/4.3.1
```

### 10.1.7 Customer's Last Login Time

The FDX API client can optionally supply the last time the customer
logged into the Data Aggregator product if this data is available.

```http
FDX-CustomerLastLoggedTime: Tue, 11 Sep 2012 19:43:31 GMT
```

### 10.1.8 Customer's IP Address

The FDX API client optionally can supply the customer's IP address if
this data is available or applicable.

```http
FDX-CustomerIPAdress: 0.0.0.0
```

### 10.1.9 Interaction Tracking

The FDX API client uses the FDX `API-InteractionId` request
header to inform server of an interaction tracing identifier. A FDX
`API-InteractionId` is unique to an FDX API client instance. The
FDX `API-InteractionId` allows support people to trace a full path of
interactions through multiple sub-systems. The FDX API server must
include the value of this header and the client identifier in any logs.

```http
FDX-InteractionId: byx24A1a111
```

The FDX API server uses the FDX `API-InteractionId` response
header to inform the client of the response FDX `API-InteractionId`. The
FDX `API-InteractionId` value must be same as the corresponding client
request header value.

```http
FDX-InteractionId: byx24A1a111
```

### 10.1.10 Financial Institution Identification

If the FDX API service is provided by a service bureau which uses the
same end point for multiple institutions, the FDX API client must
provide a header the identifies the desired financial institution. The service
bureau defines this value. For example, it is often the financial institution's
routing number (RTN).

```http
FDX-FinancialId: 123456789
```

## 10.2 Errors

When FDX API servers are unable to fulfill a request, they should send
Error Entity as the response payload along with an appropriate HTTP Status Code.
Error messages should contain just enough information for an end user to
understand what went wrong without compromising security.

| **Error Code** | **Error Message** | **HTTP Status Code** |
|---|---|---|
| 501 | Subsystem unavailable | 500 |
| 601 | Customer not found | 404 |
| 602 | Customer not authorized | 401 |
| 701 | Account not found | 404 |
| 702 | Invalid start or end date | 400 |
| 703 | Invalid date range | 400 |
| 901 | Source account not found | 404 |
| 902 | Source account closed | 404 |
| 903 | Source account not authorized for transfer | 401 |
| 904 | Destination account not found | 404 |
| 905 | Destination account closed | 404 |
| 906 | Destination account not authorized for transfer | 401 |
| 907 | Invalid amount | 404 |
| 908 | Duplicate transfer request | 409 |
| 909 | Transfer not available due to end of day processing | 503 |
| 910 | Insufficient funds | 400 |
| 911 | Transaction limit exceeded | 400 |
| 950 | Transfer not found | 404 |


# 11. Pagination

The FDX API uses cursor-based pagination by leveraging an opaque cursor in
the response that can point to an offset in the collection of records being
paginated. This leverages the defacto pagination query parameters in the request:

* `limit` an optional field to specify the maximum number of elements that the consumer wishes to receive in a single call. Providers should implement reasonable defaults and maximum. Providers can use time based limits where the result set might be less then the limit and each page could contain a different number of elements.
* `offset` an optional field to specify an opaque cursor used to retrieve a portion of the requested data

The response will be a "mixin" with `PaginatedArray`. The response will contain a `links` key of type `PageMetadataLinks` and a `page` key of type `PageMetadata`. The key fields returned are as follow:

* `page.nextOffset` is an opaque identifier used to retrieve the next page of results
* `page.prevOffset` is an opaque identifier used to retrieve the previous page of results
* `page.totalElements` contains the number of total elements matching the query
* `links.next.href` represents the URL used to retrieve the next page. This URL should contain the original parameters of the search query such as `startDate` and `endDate` if applicable
* `links.prev.href` represents the URL used to retrieve the previous page. This URL should contain the original parameters of the search query such as `startDate` and `endDate` if applicable

The client should look for `page.nextOffset` or `links.next.href` to determine if additional API calls are required to retrieve the data requested. The client should continue to make API calls until the key is no longer returned in the response.

*Note*
* The offset query parameter is a cursor for the collection to index from. It could be number or an opaque string. If it is a number it is SHOULD use a 0 based index.
* The optional limit query parameter indicates the number of records that is requested by the client. The API server SHOULD have reasonable defaults.

## 11.1 Sample implementation

```http
GET /accounts?limit=10
```

### 11.1.1 Initial Response

```json
{
  "page":{
    "nextOffset": "nextoffset@10",
    "totalElements": 100
  },
  "links":{
    "next": {"href":"/accounts?offset=nextoffset@11&limit=10"}
  },
  "accounts":[
    {"accountId": "1", "nickname": "Account 1"},
    {"accountId": "2", "nickname": "Account 2"},
    {"accountId": "3", "nickname": "Account 3"},
    {"accountId": "4", "nickname": "Account 4"},
    {"accountId": "5", "nickname": "Account 5"},
    {"accountId": "6", "nickname": "Account 6"},
    {"accountId": "7", "nickname": "Account 7"},
    {"accountId": "8", "nickname": "Account 8"},
    {"accountId": "9", "nickname": "Account 9"},
    {"accountId": "10", "nickname": "Account 10"}
  ]
}
```

### 11.1.2 Final Response

```json
{
  "page":{
    "prevOffset": "prevoffset@81",
    "totalElements": 100
  },
  "links":{
    "prev": {"href":"/accounts?offset=prevoffset@81&limit=10"}
  },
  "accounts":[
    {"accountId": "91", "nickname": "Account 91"},
    {"accountId": "92", "nickname": "Account 92"},
    {"accountId": "93", "nickname": "Account 93"},
    {"accountId": "94", "nickname": "Account 94"},
    {"accountId": "95", "nickname": "Account 95"},
    {"accountId": "96", "nickname": "Account 96"},
    {"accountId": "97", "nickname": "Account 97"},
    {"accountId": "98", "nickname": "Account 98"},
    {"accountId": "99", "nickname": "Account 99"},
    {"accountId": "100", "nickname": "Account 100"},
  ]
}
```

* Since this is the last page, `page.nextOffset` and `links.next.href` are not returned in the response.  This indicates to the client that no further pages are available.
* The response attribute prev identifies the previous page url and offset.

# 12. Resources

When implementing FDX API, client and server maintainers must agree on
the data service endpoint. All resource URIs may be prefixed by a base URI, for
example <https://api.fi.com/fdx/v4>. The base URI **should** include the version of FDX API that the server implements.

## 12.1 /accounts

| Attribute | Value |
|---|---|
|Summary|Search for accounts|
| Description | Query all information for a set of accounts provided in the payload |
| Operation Id|searchForAccounts|
| Methods | GET /accounts/{accountIds}|
| |<table><tr><th>Parameter</th><th>Type</th><th>Description</th></tr><tr><td>accountIds</td><td>array of string</td><td>Comma separated list of account ids</td></tr><tr><td>startTime</td><td>array of date</td><td>Start time for use in retrieval of transactions</td></tr><tr><td>endTime</td><td>array of date</td><td>End time for use in retrieval of transactions</td></tr><tr><td>resultType</td><td>string</td><td>Flag to indicate if you want a lightweight array of descriptors or full account details. If set to 'lightweight', should only return the fields associated with the 'AccountDescriptor' entity. This is not required, but defaults to lightweight.<br>values: lightweight, details</td></tr><tr><td>offset</td><td>string</td><td>Opaque cursor used by the provider to send the next set of records</td></tr><tr><td>limit</td><td>integer</td><td>Number of elements that the consumer wishes to receive - Providers should implement reasonable defaults and maximum</td></tr></table> |
| Request Formats          | application/json |
| Response Formats         | application/json |
| Response                 | Array of Accounts (DepositAccount, LoanAccount, LocAccount, InvestmentAccount, or AnnuityAccount) |
| Sample Request/Response  | Please see accompanying FDX API Specification schema document fdxapi4.yaml |

## 12.2 /accounts/{accountId}

| Attribute | Value |
|---|---|
|Summary|Get an account|
| Description | Get a specific account |
| Operation Id | getAccount |
| Methods | GET /accounts/{accountId}|
| |<table><tr><th>Parameter</th><th>Type</th><th>Description</th></tr><tr><td>accountId<br>*required*</td><td>string</td><td>Account identifier</td></tr></table> |
| Request Formats | application/json |
| Response Formats | application/json|
| Response | This can be one of LoanAccount, DepositAccount, LocAccount, InvestmentAccount or AnnuityAccount. |
| Sample Request/Response | Please see accompanying FDX API Specification schema document fdxapi4.yaml |

## 12.3 /accounts/{accountId}/statements/{statementId}

 | Attribute | Value |
|---|---|
| Summary | Get an account statement |
| Description | Gets account statement |
| Operation Id | getAccountStatement |
| Methods | GET /accounts/{accountId}/statements/{statementId} |
| |<table><tr><th>Parameter</th><th>Type</th><th>Description</th></tr><tr><td>accountId<br>*required*</td><td>string</td><td>Account identifier</td></tr><tr><td>statementId<br>*required*</td><td>string</td><td>Statement identifier</td></tr><tr><td>Accept</td><td>ContentTypes</td><td>Document format</td></tr></table> |
| Request Formats | application/json |
| Response Formats | application/json, application/pdf, image/gif, image/jpeg, image/png, image/tiff |
| Response | An image of an account statement (pdf, gif, jpeg, png, tiff), or an error message (json) |
| Sample Request/Response | Please see accompanying FDX API Specification schema document fdxapi4.yaml |

## 12.4 /accounts/{accountId}/statements

| Attribute | Value |
|---|---|
| Summary | Search for statements |
| Description | Get account statements |
| Operation Id | searchForAccountStatements |
| Methods | GET /accounts/{accountId}/statements?startTime=value1&endTime=value2 |
| |<table><tr><th>Parameter</th><th>Type</th><th>Description</th></tr><tr><td>accountId<br>*required*</td><td>string</td><td>Account identifier</td></tr><tr><td>startTime</td><td>date</td><td>Start time for use in retrieval of transactions</td></tr><tr><td>endTime</td><td>date</td><td>End time for use in retrieval of transactions</td></tr><tr><td>offset</td><td>string</td><td>Opaque cursor used by the provider to send the next set of records</td></tr><tr><td>limit</td><td>integer</td><td>Number of elements that the consumer wishes to receive - Providers should implement reasonable defaults and maximum</td></tr></table> |
| Request Formats | application/json |
| Response Formats | application/json |
| Response | Paginated list of available statements |
| Sample Request/Response | Please see accompanying FDX API Specification schema document fdxapi4.yaml |

## 12.5 /accounts/{accountId}/transaction-images/{imageId}

| Attribute | Value |
|---|---|
| Summary | Get account transaction image |
| Description | Get account transaction image
| Operation Id | getAccountTransactionImages |
| Methods | GET /accounts/{accountId}/transaction-images/{imageId} |
| |<table><tr><th>Parameter</th><th>Type</th><th>Description</th></tr><tr><td>accountId<br>*required*</td><td>string</td><td>Account identifier</td></tr><tr><td>imageId<br>*required*</td><td>string</td><td>Image identifier</td></tr></table> |
| Request Formats | application/json |
| Response Formats | application/pdf, image/gif, image/jpeg, image/png, image/tiff |
| Response | An image of a transaction (such as a scanned check) |
| Sample Request/Response | Please see accompanying FDX API Specification schema document fdxapi4.yaml |

## 12.6 /accounts/{accountId}/transactions

| Attribute | Value |
|---|---|
| Summary | Search for account transactions |
| Description | Search for account transactions |
| Operation Id | searchForAccountTransactions |
| Methods | GET /accounts/{accountId}/transactions?startTime=value1&endTime=value2 |
| |<table><tr><th>Parameter</th><th>Type</th><th>Description</th></tr><tr><td>accountId<br>*required*</td><td>string</td><td>Account identifier</td></tr><tr><td>startTime</td><td>date</td><td>Start time for use in retrieval of transactions</td></tr><tr><td>endTime</td><td>date</td><td>End time for use in retrieval of transactions</td></tr><tr><td>offset</td><td>string</td><td>Opaque cursor used by the provider to send the next set of records</td></tr><tr><td>limit</td><td>integer</td><td>Number of elements that the consumer wishes to receive - Providers should implement reasonable defaults and maximum</td></tr></table> |
| Request Formats | application/json |
| Response Formats | application/json |
| Response | Paged collection of transactions, which can be one of DepositTransaction, LoanTransaction, LocTransaction or InvestmentTransaction
| Sample Request/Response | Please see accompanying FDX API Specification schema document fdxapi4.yaml |

## 12.7 /availability

| Attribute | Value |
|---|---|
| Summary | Get API availability |
| Description | Get information about this API's availability
| Operation Id | getAvailability |
| Methods | GET /availability |
| Response Formats | application/json |
| Response | Availability |
| Sample Request/Response | Please see accompanying FDX API Specification schema document fdxapi4.yaml |

## 12.8 /capability

| Attribute | Value |
|---|---|
| Summary | Get API capability |
| Description | Get information about this API's capability |
| Operation Id | getCapability |
| Methods | GET /capability |
| Response Formats | application/json |
| Response | Capability |
| Sample Request/Response | Please see accompanying FDX API Specification schema document fdxapi4.yaml |

## 12.9 /customers/current

| Attribute | Value |
|---|---|
| Summary | Get current authenticated customer information |
| Description | Get information about the customer within the authorization scope. |
| Operation Id | getCustomerInfo |
| Methods | GET /customers/current |
| Response Formats | application/json |
| Response | Customer |
| Sample Request/Response | Please see accompanying FDX API Specification schema document fdxapi4.yaml |

## 12.10 /transfers

| Attribute | Value |
|---|---|
| Summary | Request account transfer |
| Description | Create a transfer between accounts |
| Operation Id | requestAccountTransfer |
| Methods | POST /transfers |
| |<table><tr><th>Parameter</th><th>Type</th><th>Description</th></tr><tr><td>body<br>*required*</td><td>Transfer</td><td>Transfer Entity</td></tr></table> |
| Request Formats | application/json |
| Response Formats | application/json |
| Response | TransferStatus |
| Sample Request/Response | Please see accompanying FDX API Specification schema document fdxapi4.yaml |

## 12.12 /transfers/{transferId}/status

| Attribute | Value |
|---|---|
| Summary | Get transfer status |
| Description | Get status of a transfer between accounts |
| Operation Id | getTransferStatus |
| Methods | GET /transfers/{transferId}/status |
| | <table><tr><th>Parameter</th><th>Type</th><th>Description</th></tr><tr><td>transferId<br>*required*</td><td>string</td><td>Transfer Identifier</td></tr></table> |
| Request Formats | application/json |
| Response Formats | application/json |
| Response | TransferStatus |
| Sample Request/Response | Please see accompanying FDX API Specification schema document fdxapi4.yaml |

## 12.13 /tax-forms

| Attribute | Value |
|---|---|
| Summary | Search tax forms |
| Description: | Get the full lists of tax data and tax form images available for a specific year for the current authorized customer |
| Operation Id | searchForTaxForms |
| Methods | GET /tax-forms |
| | <table><tr><th>Parameter</th><th>Type</th><th>Description</th></tr><tr><td>taxYears</td><td>string</td><td>Tax year</td></tr><tr><td>taxForms</td><td>Array of TaxFormType</td><td>One or more tax form names for the specific forms being requested. Comma separated.</td></tr><tr><td>contentTypes</td><td>Array of ContentTypes</td><td>One or more content-types to search for. Comma separated</td></tr><tr><td>resultType</td><td>string</td><td>enum: [lightweight, details]<br/>Flag to indicate if you want a lightweight array of Tax entities or the full tax form details. If set to 'lightweight', should only return the fields associated with the 'Tax' entity. This field is not required, but defaults to lightweight</td></tr></table> |
| Request Formats | application/json |
| Response Formats | application/json |
| Response | TaxDataList |
| Sample Request/Response | Please see accompanying FDX API Specification schema document fdxapi4.yaml |

## 12.14 /tax-forms/{taxFormId}

| Attribute | Value |
|---|---|
| Summary | Retrieve tax form |
| Description | The form image or TaxData as json for a single tax form for the customer |
| Operation Id | getTaxForm |
| Methods | GET /tax-forms/{taxFormId} |
| |<table><tr><th>Parameter</th><th>Type</th><th>Description</th></tr><tr><td>taxFormId<br>*required*</td><td>integer</td><td>Form Id</td></tr><tr><td>Accept</td><td>ContentTypes</td><td></td></tr></table> |
| Response | The form image or TaxData as json for a single tax form for the customer |
| Sample Request/Response | Please see accompanying FDX API Specification schema document fdxapi4.yaml |

## 12.15 POST /tax-forms

| Attribute | Value |
|---|---|
| Summary | Create tax form |
| Description | Submit the data for a specific tax form |
| Operation Id | createTaxForm |
| Methods | POST /tax-forms |
| | <table><tr><th>Parameter</th><th>Type</th><th>Description</th></tr><tr><td>body</td><td>array of TaxData</td><td>The full data contents of the form including the indexing metadata values</td></tr></table> |
| Request Formats | application/json |
| Response Formats | application/json |
| Response | Array of TaxData |
| Sample Request/Response | Please see accompanying FDX API Specification schema document fdxapi4.yaml |

## 12.16 PUT /tax-forms/{taxFormId}

| Attribute | Value |
|---|---|
| Summary | Update tax form |
| Description | Update tax document. Allows you to upload and replace binaries or json document |
| Operation Id | updateTaxForm |
| Methods | PUT /tax-forms/{taxFormId} |
| |<table><tr><th>Parameter</th><th>Type</th><th>Description</th></tr><tr><td>taxFormId<br>*required*</td><td>integer</td><td>Form Id</td></tr></table> |
| Request Formats | application/json, application/pdf, image/gif, image/jpeg, image/png, image/tiff |
| Response | The form image or TaxData as json for a single tax form for the customer |
| Sample Request/Response | Please see accompanying FDX API Specification schema document fdxapi4.yaml |


# 13. Versioning

APIs should be designed for change to expose new behavior or data. Non-breaking (backward compatible) service changes are denoted by minor versions (e.g. v1 moves to v1.1) while breaking changes (not backward compatible) are denoted by a major version (e.g. v1 moves to v2). When minor versions of services are released they are backward compatible so the services released in place of the existing service. When major versions of services are released there are multiple versions of the same service available at the same time. The reason for a new service version is because some major piece of functionality has changed in the service and consumer of the previous version should be motivated to adopt the new version. We realize that quick movement to the new service will not always happen. Therefore, we recommend that no more than three approved major versions of a service exist at any one time. When a fourth major version is released into production the oldest version will be considered deprecated and scheduled for retirement within a period not to exceed 12 months.

# 14. Entities

Entities are specified in the accompanying FDX API Specification document fdxapi4.yaml.

## 14.1 Account Entity

An abstract account entity that concrete account entities extend. Extends and inherits all fields from AccountDescriptor.

| **Field** | **Type** | **Description** |
|---|---|---|
| **parentAccountId** | Identifier | Long-term persistent identity of the parent account. This is used to group accounts. |
| **lineOfBusiness** | string | The line of business, such as consumer, consumer joint, small business, corporate, etc. |
| **routingTransitNumber** | string | Routing transit number (RTN) associated with account number at owning institution. |
| **balanceType** | BalanceType | ASSET (positive transaction amount increases balance), LIABILITY (positive transaction amount decreases balance) |
| **interestRate** | Number | Interest Rate of Account |
| **interestRateType** | InterestRateType | FIXED or VARIABLE |
| **interestRateAsOf** | Timestamp | Date of account's interest rate |
| **priorInterestRate** | Number | Previous Interest Rate of Account |
| **transferIn** | Boolean | Account is eligible for incoming transfers |
| **transferOut** | Boolean | Account is eligible for outgoing transfers |
| **micrNumber** | String64 | MICR Number |
| **lastActivityDate** | Timestamp | Date that last transaction occurred on account |
| **transactionsIncluded** | Boolean | Default is false. If present and true, a call to retrieve transactions will not return any further details about this account. This is an optimization that allows an FDX API server to return transactions and account details in a single call. |

## 14.2 AccountDescriptor Entity

Minimal information about the account for use in lightweight arrays.

| **Field**        | **Type**             | **Description** |
|---|---|---|
| **accountId**    | Identifier           | Long-term persistent identity of the account. Not an account number. This identity must be unique to the owning institution. |
| **error**        | Error                | Present if an error was encountered while retrieving this account |
| **accountType**  | AccountType          | Account type |
| **accountNumber**  | string               | Full account number for the end user's handle for account at owning institution.      |
| **accountNumberDisplay**  | string               | Account display number for the end user's handle at owning institution. This is to be displayed by the Interface Provider.      |
| **productName**       | string        | Marketed product name for this account. Used in UIs to assist in account selection |
| **nickName**       | string        | Name given by the user. Used in UIs to assist in account selection |
| **status**       | AccountStatus        | OPEN, CLOSED, PENDINGOPEN, PENDINGCLOSE, DELINQUENT, PAID, NEGATIVECURRENTBALANCE. |
| **description**  | string               | Description of account |
| **currency**  | Currency               |  |
| **fiAttributes** | Array of FiAttribute | Array of FI-specific attributes |

## 14.3 AccountDescriptorList Entity

A lightweight array of accounts.

| **Field**| **Type**| **Description** |
|---|---|---|
| **accountDescriptor** | Array of AccountDescriptor ||

## 14.4 Accounts Entity

An optionally paginated array of accounts.

| **Field** | **Type** | **Description** |
|---|---|---|
| **page** | PageMetadata||
| **links** | PageMetadataLinks ||
| **accounts**   | zero or more DepositAccount, InvestmentAccount, LoanAccount, LocAccount, AnnuityAccount, InsuranceAccount   | An array of accounts with entity types dependent on the account type (deposit, investment, loan, line of credit, annuity or insurance) |

## 14.5 AccountDetailsRequest Entity

| **Field** | **Type** | **Description** |
|---|---|---|
| **accounts** | Array of SingleAccountDetailsRequest ||

## 14.6 Availability Entity

| **Field** | **Type** | **Description** |
|---|---|---|
| **currentStatus**       | string                       | |
| **currentStatusDesc**   | string                       | |
| **plannedAvailability** | Array of PlannedAvailability | |

## 14.7 Capability Entity

Describes capabilities of this implementation of FDX API.

| **Field** | **Type** | **Description** |
|---|---|---|
| **allowedConnections**   | int | Number of concurrent connections allowed for this client |
| **activeConnections**    | int | Current number of active connections for this client |
| **supportsCustomer**     | Boolean | Capable of responding to customer queries. Defaults to false. |
| **supportsAccounts**     | Boolean | Capable of responding to accounts and account queries. Defaults to false.     |
| **supportsTransactions** | Boolean | Capable of responding to transaction queries. Defaults to false. |
| **supportsStatements**   | Boolean | Capable of responding to statements and statement queries. Defaults to false. |
| **supportsImage**        | Boolean | Capable of responding to image queries. Defaults to false. |
| **supportsBillPay** | Boolean | Capable of supporting a bill payment request. Defaults to false. |
| **supportsTransfer**     | Boolean       | Capable of supporting a transfer request. Defaults to false. |
| **messageFormat**        | MessageFormat | Defaults to JSON. |
| **transferCutOffTime**   | Timestamp     | Cut off time for transfers. Required if transfers are supported.              |
| **billPayCutOffTime**    | Timestamp     | Cut off time for bill payment. Required if bill payment is supported. |

## 14.8 Contribution Entity

| **Field** | **Type** | **Description** |
|---|---|---|
| **securityId** | string | Unique identifier of security |
| **securityIdType** | SecurityIdType | Security identifier type |
| **employerMatchPercentage** | Number | Employer contribution match percentage |
| **employerMatchAmount** | Number | Employer contribution match amount |
| **employeePreTaxAmount** | Number | Employee pre-tax contribution amount |
| **employeePreTaxPercentage** | Number | Employee pre-tax contribution percentage |
| **employeeAfterTaxAmount** | Number | Employee after tax contribution amount |
| **employeeAfterTaxPercentage** | Number | Employee after tax contribution percentage |
| **employeeDeferPreTaxAmount** | Number | Employee defer pre-tax contribution match amount |
| **employeeDeferPreTaxPercentage**  | Number | Employee defer pre-tax contribution match percentage |
| **employeeYearToDate** | Number | Employee total year to date contribution |
| **employerYearToDate** | Number | Employer total year to date contribution |
| **rolloverContributionPercentage** | Number | Rollover contribution percentage |
| **rolloverContributionAmount** | Number | Rollover contribution Amount |

## 14.9 Currency Entity

| **Field** | **Type**    | **Description** |
|---|---|---|
| **currencyRate** | Number | Currency rate between original and converted currency |
| **currencyCode** | Iso4217Code | ISO 4217 currency code |
| **originalCurrencyCode** | Iso4217Code | Original ISO 4217 currency code |

## 14.10 Customer Entity

| **Field** | **Type** | **Description** |
|---|---|---|
| **customerId** | Identifier | Long-term persistent identity of the customer. This identity must be unique to the owning institution. |
| **name** | CustomerName | The customer's name |
| **dateOfBirth**  | Timestamp | The customer's date of birth |
| **taxId** | string | The SSN or TIN associated with this customer |
| **governmentId** | string | A federal (such as passport) or state (such as driver's license) issued identifier |
| **email** | Array of string | An array of the customer's electronic mail addresses |
| **addresses** | Array of DeliveryAddress | An array of the customer's physical mail addresses |
| **telephones** | Array of TelephoneNumber | An array of the customer's telephone numbers |

## 14.11 CustomerName Entity

The name of an individual in their role as a customer.  Inherits and extends all of IndividualName.

| **Field**   | **Type** | **Description**       |
|-------------|----------|-----------------------|
| **prefix**  | string   | Name prefix, e.g. Mr. |
| **company** | string   | Company name          |

## 14.12 DebtSecurity Entity

| **Field** | **Type** | **Description** |
|---|---|---|
| **parValue** | Number | Par value amount |
| **debtType** | DebtType | Debt type (COUPON, ZERO) |
| **debtClass** | DebtClass | Classification of debt (TREASURY, MUNICIPAL, CORPORATE, OTHER) |
| **couponRate** | Number | Bond coupon rate for next closest call date |
| **couponDate** | Timestamp | Maturity date for next coupon |
| **couponMatureFrequency** | CouponMatureFrequency | When coupons mature. One of the following values: MONTHLY, QUARTERLY, SEMIANNUAL, ANNUAL, or OTHER |
| **callPrice** | Number | Bond call price |
| **yieldToCall** | Number | Yield to next call |
| **callDate** | Timestamp | Next call date |
| **callType** | CallType | Type of next call. CALL, PUT, PREFUND, MATURITY |
| **yieldToMaturity** | Number | Yield to maturity |
| **bondMaturityDate** | Timestamp | Bond Maturity date |

## 14.13 DeliveryAddress Entity

A delivery address and its location type.  Inherits and extends all of Address.

| **Field**   | **Type** | **Description** |
|---|---|---|
| **type** | DeliveryAddressType | HOME, BUSINESS, MAILING |

## 14.14 DepositAccount Entity

Extends and inherits all fields from Account.

| **Field** | **Type** | **Description** |
|---|---|---|
| **balanceAsOf** | Timestamp | As-of date of balances |
| **currentBalance** | Number | Balance of funds in account |
| **openingDayBalance** | Number | Day's opening fund balance |
| **availableBalance** | Number | Balance of funds available for use |
| **annualPercentageYield** | Number | Annual Percentage Yield |
| **interestYtd** | Number | YTD Interest |
| **term** | int | Term of CD in months |
| **maturityDate** | Timestamp | Maturity date for CDs |
| **transactions** | Array of DepositTransaction | |

## 14.15 DepositTransaction Entity

Extends and inherits all fields from Transaction.

| **Field** | **Type** | **Description** |
|---|---|---|
| **transactionType** | DepositTransactionType | CHECK, WITHDRAWAL, TRANSFER, POSDEBIT, ATMWITHDRAWAL, BILLPAYMENT, FEE, DEPOSIT, ADJUSTMENT, INTEREST, DIVIDEND, DIRECTDEPOSIT, ATMDEPOSIT, POSCREDIT |
| **payee** | String255 | Payee name |
| **checkNumber** | int | Check number |

## 14.16 Error Entity

An error entity which can be used at the API level for error responses or at the account level to indicate a problem specific to a particular account.

| **Field** | **Type** | **Description** |
|---|---|---|
| **code**    | string   | Long term persistent identifier which can be used to trace error condition back to log information |
| **message** | string   | End user displayable information which might help the customer diagnose an error |

## 14.17 FiAttribute Entity

Financial platform provider-specific attribute.

| **Field** | **Type** | **Description**    |
|-----------|----------|--------------------|
| **name**  | string   | Name of attribute  |
| **value** | string   | Value of attribute |

## 14.18 FiPortion Entity

| **Field** | **Type** | **Description** |
|---|---|---|
| **assetClass** | string   | FI-specific asset class |
| **percent**    | Number   | Percentage of asset class that falls under this asset |

## 14.19 Holding Entity

| **Field** | **Type** | **Description** |
|---|---|---|
| **holdingId** | Identifier | Long term persistent identity of the holding |
| **securityId** | string | Unique identifier of security |
| **securityIdType** | SecurityIdType | Security identifier type |
| **holdingName** | string | Holding name or security name |
| **holdingType** | HoldingType | STOCK, BOND, MUTUALFUND, CD, ANNUITY, OPTION, OTHER |
| **holdingSubType** | HoldingSubType | MONEYMARKET, CASH |
| **positionType** | PositionType | LONG, SHORT |
| **heldInAccount** | HeldInAccount | Sub-account CASH, MARGIN, SHORT, OTHER |
| **description** | string | The description of the holding |
| **symbol** | string | Ticker / Market symbol |
| **originalPurchaseDate** | Timestamp | Date of original purchase |
| **purchasedPrice** | Number | Price of holding at the time of purchase |
| **currentUnitPrice** | Number | Current unit price |
| **changeInPrice** | Number | Change in current price compared to previous day's close |
| **currentUnitPriceDate** | Timestamp | Current unit price as of date |
| **units** | Number | Required for stock, mutual funds. Number of shares (with decimals). |
| **marketValue** | Number | Market value at the time of data retrieved |
| **faceValue** | Number | Required for bonds. Face value at the time of data retrieved. |
| **averageCost** | Boolean | Cost is average of all purchases for holding |
| **cashAccount** | Boolean | If true, indicates that this holding is used to maintain proceeds from sales, dividends, and other cash postings to the investment account |
| **rate** | Number | For CDs, bonds, and other rate based holdings |
| **expirationDate** | Timestamp | For CDs, bonds, and other time-based holdings |
| **inv401kSource** | Inv401kSourceType | Source for money for this security. PRETAX, AFTERTAX, MATCH, PROFITSHARING, ROLLOVER, OTHERVEST, OTHERNONVEST |
| **currency** | Currency | Currency information if it is different from Account entity |
| **assetClasses** | Array of Portion | Percent breakdown by asset class |
| **fiAssetClasses** | Array of FiPortion | Percent breakdown by FI-specific asset class percentage breakdown |
| **fiAttributes** | Array of FiAttribute | Array of FI-specific attributes |
| **taxLots** | Array of TaxLot | Breakdown by tax lot |
| one of **mutualFundSecurity**, **optionSecurity**, **otherSecurity**, **stockSecurity**, **sweepSecurity**, or **debtSecurity** | MutualFundSecurity, OptionSecurity, OtherSecurity, StockSecurity, SweepSecurity, or DebtSecurity | Information about the security specific to the type of security |

## 14.20 InvestmentAccount Entity

Extends and inherits all fields from Account.

| **Field** | **Type** | **Description** |
|---|---|---|
| **balanceAsOf** | Timestamp | As-of date for balances |
| **allowedCheckWriting**  | Boolean | Check writing privileges |
| **allowedOptionTrade**    | Boolean | Allowed to trade options |
| **currentValue** | Number | Total current value of all investments |
| **holdings** | Array of Holding | Array of holdings |
| **openOrders** | Array of OpenOrder | Array of open orders |
| **contribution** | Array of Contribution | Describes how new contributions are distributed among the available securities |
| **vesting** | Array of Vesting | Provides the past, present, and future vesting schedule and percentages |
| **investmentLoans** | Array of InvestmentLoan | Array of investment loans |
| **availableCashBalance** | Number | Cash balance across all sub-accounts. Should include sweep funds. |
| **margin** | Boolean | Margin trading is allowed |
| **marginBalance** | Number | Margin balance |
| **shortBalance** | Number | Short balance |
| **rolloverAmount** | Number | Rollover amount |
| **employerName** | string | Name of the employer in investment 401k Plan |
| **brokerId** | string | Unique identifier FI |
| **planId** | string | Plan number for Investment 401k plan |
| **calendarYearFor401k**  | Timestamp | Date for this calendar year for 401K account |
| **balanceList** | Array of InvestmentBalance | Balance List. Name value pair aggregate. |
| **dailyChange** | Number | Daily change |
| **percentageChange** | Number | Percentage change |
| **transactions** | Array of InvestmentTransaction | Array of Investment Transactions |
| **pensionSource** | Array of PensionSource | Array of Pension Source |

## 14.21 InvestmentBalance Entity

| **Field** | **Type** | **Description** |
|---|---|---|
| **balanceName** | string | Name of the balance |
| **balanceDescription** | string | Description of balance |
| **balanceType** | InvestmentBalanceType | AMOUNT, PERCENTAGE |
| **balanceValue** | Number | Value of balance name |
| **balanceDate** | Timestamp | Date as of this balance |
| **currency** | Currency | Currency if different from that of account |

## 14.22 InvestmentLoan Entity

| **Field** | **Type** | **Description** |
|---|---|---|
| **loanId** | string | Unique identifier for this loan |
| **loanDescription** | string | Description |
| **initialLoanBalance** | Number | Initial loan balance amount |
| **loanStartDate** | Timestamp | Start date of the loan |
| **currentLoanBalance** | Number | Current loan principal balance amount |
| **dateAsOf** | Timestamp | Date and time of current loan balance |
| **loanRate** | Number | Loan annual interest rate for the loan |
| **loanPaymentAmount** | Number | Loan payment amount |
| **loanPaymentFrequency** | LoanPaymentFrequency | WEEKLY, BIWEEKLY, TWICEMONTHLY, MONTHLY,FOURWEEKS, BIMONTHLY, QUARTERLY, SEMIANNUALLY, ANNUALLY, OTHER |
| **loanPaymentInitial** | Number | Initial number of loan payments |
| **loanPaymentsRemaining** | int | Remaining number of loan payments |
| **loanMaturityDate** | Timestamp | Expected loan end date |
| **loanInterestToDate** | Number | Total interest paid to date on this loan |
| **loanTotalProjectedInterest** | Number | Total projected interest to be paid on this loan |
| **loanNextPaymentDate** | Timestamp | The next payment date for the loan |

## 14.23 InvestmentTransaction Entity

Extends and inherits all fields from Transaction.

| **Field** | **Type** | **Description** |
|---|---|---|
| **transactionType** | InvestmentTransactionType | PURCHASED, SOLD, PURCHASEDTOCOVER, ADJUSTMENT, PURCHASETOOPEN, PURCHASETOCLOSE, SOLDTOOPEN, SOLDTOCLOSE, INTEREST, MARGININTEREST, REINVESTOFINCOME, RETURNOFCAPITAL, TRANSFER, CONTRIBUTION, FEE, OPTIONEXERCISE, OPTIONEXPIRATION, DIVIDEND, DIVIDENDREINVEST, SPLIT, CLOSURE, INCOME, EXPENSE, CLOSUREOPT, INVEXPENSE, JRNLSEC, JRNLFUND, OTHER, DIV, SRVCHG, DEP, DEPOSIT, ATM, POS, XFER, CHECK, PAYMENT, CASH, DIRECTDEP, DIRECTDEBIT, REPEATPMT |
| **shares** | Number | Required for stock, mutual funds. Number of shares (with decimals). Negative numbers indicate securities are being removed from the account. |
| **faceValue** | Number | Cash value for bonds |
| **price** | Number | Unit purchase price |
| **securityId** | string | Unique identifier of security |
| **securityIdType** | SecurityIdType | Security identifier type |
| **securityType** | SecurityType | STOCK, MUTUALFUND, DEBT, OPTION, SWEEP, OTHER, BOND |
| **symbol** | string | Ticker symbol |
| **markup** | Number | Portion of unit price that is attributed to the dealer markup |
| **commission** | Number | Transaction commission |
| **taxes** | Number | Taxes on the trade |
| **fees** | Number | Fees applied to the trade |
| **load** | Number | Load on the transaction |
| **inv401kSource** | Inv401kSourceType         | Source of money. PRETAX, AFTERTAX, MATCH,PROFITSHARING, ROLLOVER, OTHERVEST, OTHERNONVEST |
| **confirmationNumber** | string | Confirmation number of the transaction |
| **fractionalCash** | Number | Cash for fractional units (used for stock splits) |
| **incomeType** | IncomeType | Type of investment income: CGLONG (capital gains-long term), CGSHORT (capital gains-short term), MISC |
| **oldUnits** | Number | Number of shares before split |
| **splitRatioNumerator** | Number | Split ratio numerator |
| **splitRatioDenominator** | Number | Split ratio denominator |
| **newUnits** | Number | Number of shares after split |
| **subAccountSec** | SubAccountType | Sub-account security Type: CASH, MARGIN, SHORT and OTHERS |
| **subAccountFund** | SubAccountType | From which account money came in: CASH, MARGIN, SHORT and OTHERS |
| **loanId** | string | For 401k accounts only. This indicates the transaction was due to a loan or a loan repayment. |
| **loanPrincipal** | Number | How much loan pre-payment is principal |
| **loanInterest** | Number | How much loan pre-payment is interest |
| **payrollDate** | Timestamp | The date for the 401k transaction was obtained in payroll |
| **priorYearContrib** | Boolean | Indicates this buy was made using prior years contribution. TRUE or FALSE |
| **withholding** | Number | Federal tax withholding |
| **taxExempt** | Boolean | Tax-exempt transaction TRUE or FALSE |
| **gain** | Number | For sales |
| **stateWithholding** | Number | State tax withholding |
| **penalty** | Number | Indicates amount withheld due to a penalty |
| **runningBalance** | Number | Running balance of the position |
| **unitPrice** | Number | Price per commonly-quoted unit. Does not include markup/markdown, unitprice. Share price for stocks, mutual funds, and others. Percentage of par for bonds. Per share (not contract) for options. |
| **units** | Number | For security-based actions other than stock splits, quantity. Shares for stocks, mutual funds, and others. Face value for bonds. Contracts for options. |
| **unitType** | UnitType | SHARES, CURRENCY |
| **transactionReason** | TransactionReason | Reason for this transaction; CALL (the debt was called), SELL (the debt was sold), MATURITY (the debt reached maturity) |
| **accruedInterest** | Amount | Accrued Interest |
| **transferAction** | string | Transfer direction [IN or OUT] |
| **positionType** | PositionType | LONG, SHORT |

## 14.24 LineItem Entity

| **Field** | **Type** | **Description** |
|---|---|---|
| **description** | string | The description of the line item |
| **amount** | Number | The amount of money attributable to this line item |
| **checkNumber** | int | Check number |
| **memo** | String255 | Secondary item description |
| **reference** | string | A reference number |
| **imageIds** | Array of string | Array of image identifiers (unique to transaction) used to retrieve images of check or transaction receipt |
| **links** | Array of HateoasLink | Array of Image Identifiers (unique to Transaction) used to retrieve Images of                 check or transaction receipt |

## 14.25 LoanAccount Entity

Extends and inherits all fields from Account.

| **Field** | **Type** | **Description** |
|---|---|---|
| **balanceAsOf** | Timestamp | As-of date for balances |
| **principalBalance** | Number | Principal balance of loan |
| **escrowBalance** | Number | Escrow balance of loan |
| **originalPrincipal** | Number | Original principal of loan |
| **originatingDate** | Timestamp | Loan origination date |
| **loanTerm** | int | Term of loan in months |
| **totalNumberOfPayments**  | int | Total number of payments |
| **nextPaymentAmount** | Number | Amount of next payment |
| **nextPaymentDate** | Timestamp | Date of next payment |
| **paymentFrequency** | PaymentFrequency | DAILY, WEEKLY, BIWEEKLY, SEMIMONTHLY, MONTHLY, SEMIANNUALLY, ANNUALLY |
| **compoundingPeriod** | CompoundingPeriod | DAILY, WEEKLY, BIWEEKLY, SEMIMONTHLY, MONTHLY, SEMIANNUALLY, ANNUALLY |
| **payoffAmount** | Number | Payoff amount |
| **lastPaymentAmount** | Number | Last payment amount |
| **lastPaymentDate** | Timestamp | Last payment date |
| **maturityDate** | Timestamp | Maturity date |
| **interestPaidYearToDate** | Number | Interest paid year to date |
| **transactions** | Array of LoanTransaction | |

## 14.26 LoanTransaction Entity

Extends and inherits all fields from Transaction.

| **Field** | **Type** | **Description** |
|---|---|---|
| **transactionType** | LoanTransactionType | PAYMENT, FEE, ADJUSTMENT, INTEREST |
| **paymentDetails**  | PaymentDetails | Breakdown of payment details |

## 14.27 LocAccount Entity

Extends and inherits all fields from Account.

| **Field** | **Type** | **Description** |
|---|---|---|
| **balanceAsOf** | Timestamp | As-of date of balances |
| **creditLine** | Number | Credit limit |
| **availableCredit** | Number | Available credit |
| **nextPaymentAmount** | Number | Amount of next payment |
| **nextPaymentDate** | Timestamp | Due date of next payment |
| **principalBalance** | Number | Principal balance |
| **currentBalance** | Number | Current balance LOC |
| **minimumPaymentAmount**  | Number | Minimum payment amount   |
| **lastPaymentAmount** | Number | Last payment amount |
| **lastPaymentDate** | Timestamp | Last payment date |
| **pastDueAmount** | Number | Past Due Amount |
| **lastStmtBalance** | Number | Last Statement Balance |
| **lastStmtDate** | Timestamp | Last Statement Date |
| **pointsAccrued** | Number | Points accrued |
| **currentRewardsBalance** | Number | Current rewards balance |
| **pointsRedeemed** | Number | Points redeemed |
| **purchasesApr** | Number | Purchases APR |
| **advancesApr** | Number | Advances APR |
| **cashAdvanceLimit** | Number | Cash advance limit |
| **availableCash** | Number | Available cash |
| **financeCharges** | Number | Finance charges |
| **transactions** | Array of LocTransaction | |

## 14.28 LocTransaction Entity

A line of credit transaction.  Extends and inherits all fields from Transaction.

| **Field** | **Type** | **Description** |
|---|---|---|
| **transactionType** | LocTransactionType | CHECK, WITHDRAWAL, PAYMENT, FEE, ADJUSTMENT, INTEREST |
| **checkNumber** | int | Check number |
| **paymentDetails**  | PaymentDetails | Breakdown of payment details |

## 14.29 MutualFundSecurity Entity

| **Field** | **Type** | **Description** |
|---|---|---|
| **mutualFundType** | MutualFundType | Mutual fund type. OPENEND, CLOSEEND, OTHER |
| **unitsStreet** | Number | Units in the FI's street name, positive quantity |
| **unitsUser** | Number | Units in user's name directly, positive quantity |
| **reinvestDividends** | Boolean | Reinvest dividends |
| **reinvestCapitalGains** | Boolean | Reinvest capital gains |
| **yield** | Number | Current yield reported as portion of the fund's assets |
| **yieldAsOfDate** | Timestamp | As-of date for yield value |

## 14.30 OpenOrder Entity

| **Field** | **Type** | **Description** |
|---|---|---|
| **orderId** | Identifier | Long term persistent identity of the order. Id for this order transaction. |
| **securityId** | string | Unique identifier of security |
| **securityIdType** | SecurityIdType | Security identifier type |
| **symbol** | string | Market symbol |
| **description** | string | Description of order |
| **units** | Number | Number of units (shares or bonds etc). |
| **orderType** | OrderType | Type of order BUY, SELL, BUYTOCOVER, BUYTOOPEN, SELLTOCOVER, SELLTOOPEN, SELLSHORT, SELLCLOSE |
| **orderDate** | Timestamp | Order date |
| **unitPrice** | Number | Unit price |
| **unitType** | UnitType | Type of unit SHARES, CURRENCY |
| **orderDuration**  | OrderDuration | This order is good for DAY, GOODTILLCANCEL, IMMEDIATE |
| **subAccount** | SubAccountType | CASH, MARGIN, SHORT, OTHER |
| **limitPrice** | Number | Limit price |
| **stopPrice** | Number | Stop price |
| **inv401kSource**  | Inv401kSourceType | For 401(k) accounts, source of money for this order. PRETAX, AFTERTAX, MATCH, PROFITSHARING, ROLLOVER, OTHERVEST, OTHERNONVEST. Default if not present is OTHERNONVEST. |

## 14.31 OptionSecurity Entity

| **Field** | **Type** | **Description** |
|---|---|---|
| **secured** | Secured | How the option is secured. NAKED, COVERED. |
| **optionType** | OptionType | Option type: PUT = put, CALL = call |
| **strikePrice** | Number | Strike price / Unit price |
| **expireDate** | Timestamp  | Expiration date of option |
| **sharesPerContract** | Number | Shares per contract |

## 14.32 OtherSecurity Entity

| **Field**           | **Type** | **Description**               |
|---------------------|----------|-------------------------------|
| **typeDescription** | string   | Description of Other Security |

## 14.33 PaymentDetails Entity

| **Field** | **Type** | **Description** |
|---|---|---|
| **principalAmount** | Number   | The amount of payment applied to principal |
| **interestAmount**  | Number   | The amount of payment applied to interest |
| **insuranceAmount** | Number   | The amount of payment applied to life/ health/accident insurance on the loan |
| **escrowAmount**    | Number   | The amount of payment applied to escrow |
| **pmiAmount**       | Number   | The amount of payment applied to PMI |
| **feesAmount**      | Number   | The amount of payment applied to fees |

## 14.34 PlannedAvailability Entity

| **Field**           | **Type**  | **Description** |
|---------------------|-----------|-----------------|
| **status**          | string    |                 |
| **statusShortDesc** | string    |                 |
| **statusStartDate** | Timestamp |                 |
| **statusEndDate**   | Timestamp |                 |

## 14.35 Portion Entity

| **Field** | **Type** | **Description** |
|---|---|---|
| **assetClass** | AssetClass | DOMESTICBOND, INTLBOND, LARGESTOCK, SMALLSTOCK, INTLSTOCK, MONEYMARKET, OTHER |
| **percent**    | Number     | Percentage of asset class that falls under this asset |

## 14.36 SingleAccountDetailsRequest Entity

| **Field** | **Type**  | **Description** |
|---|---|---|
| **accountId** | string | Account identifier |
| **startTime** | Timestamp | Start time for use in retrieval; ISO 8601 date including zone indicator or combined date time including zone indicator |
| **endTime** | Timestamp | End time for use in retrieval; ISO 8601 date including zone indicator or combined date time including zone indicator |

## 14.37 Statement Entity

| **Field** | **Type**   | **Description** |
|---|---|---|
| **accountId** | Identifier | Corresponds to AccountId in Account entity |
| **statementId** | Identifier | Long-term persistent identity of the statement |
| **statementDate** | Timestamp  | Date of the statement |
| **description** | string | Description of statement |

## 14.38 Statements Entity

Array of Statement entities, inherits and implements PaginatedArray

| **Field** | **Type** | **Description** |
|---|---|---|
| **statements** | Array of Statement | Statements |
| **self** | HateoasLink | |

## 14.39 StockSecurity Entity

| **Field** | **Type**  | **Description** |
|---|---|---|
| **unitsStreet** | Number | Units in the FI's street name, positive quantity |
| **unitsUser** | Number | Units in user's name directly, positive quantity |
| **reinvestDividends** | Boolean   | Reinvest dividends |
| **stockType** | StockType | COMMON, PREFERRED, CONVERTIBLE, OTHER |
| **yield** | Number    | Current yield |
| **yieldAsOfDate** | Timestamp | Yield as-of date |

## 14.40 SweepSecurity Entity

| **Field** | **Type**  | **Description** |
|---|---|---|
| **currentBalance** | Number | Balance of funds in account |
| **availableBalance** | Number | Balance of funds available for use |
| **balanceAsOf** | Timestamp | As-of date of balances |
| **checks** | Boolean   | Whether or not checks can be written on the account |

## 14.41 TaxLot Entity

| **Field** | **Type** | **Description** |
|---|---|---|
| **originalPurchaseDate** | Timestamp | Lot acquired date |
| **quantity** | Number | Lot quantity |
| **purchasedPrice** | Number | Original purchase price |
| **costBasis** | Number | Total amount of money spent acquiring this lot including any fees or commission expenses incurred |
| **currentValue** | Number | Lot market value |
| **positionType** | PositionType | LONG, SHORT |

## 14.42 TelephoneNumber Entity

Telephone subscriber number defined by ITU-T recommendation E.164

| **Field** | **Type** | **Description** |
|---|---|---|
| **type** | TelephoneNumberType | HOME, BUSINESS, CELL, FAX |
| **country** | String3 | Country calling codes defined by ITU-T recommendations E.123 and E.164 |
| **number**  | String15 | Telephone number |

## 14.43 Transaction Entity

| **Field** | **Type** | **Description** |
|---|---|---|
| **accountId** | Identifier | Corresponds to AccountId in Account |
| **transactionId** | Identifier | Long term persistent identity of the transaction (unique to account) |
| **referenceTransactionId** | Identifier | For reverse postings, the identity of the transaction being reversed. For the correction transaction, the identity of the reversing post. For credit card posting transactions, the identity of the authorization transaction. |
| **postedTimestamp** | Timestamp | The date and time that the transaction was posted to the account. If not provided then TransactionTimestamp can be used as PostedTimeStamp. |
| **transactionTimestamp** | Timestamp | The date and time that the transaction was added to the server backend systems |
| **description** | string | The description of the transaction |
| **memo** | String255 | Secondary transaction description |
| **debitCreditMemo** | DebitCreditMemo | DEBIT, CREDIT, MEMO |
| **category** | string | Transaction category, preferably MCC or SIC. |
| **subCategory** | string | Transaction category detail |
| **reference** | string | A tracking reference identifier |
| **status** | TransactionStatus | PENDING, MEMO, POSTED, AUTHORIZATION |
| **amount** | Number | The amount of money in the account currency |
| **foreignAmount** | Number | The amount of money in the foreign currency |
| **foreignCurrency** | Iso4217Code | The ISO 4217 code of the foreign currency |
| **imageIds** | Array of string | Array of Image Identifiers (unique to Transaction) used to retrieve Images of check or transaction receipt |
| **lineItem** | Array of LineItem | Breakdown of the transaction details |
| **fiAttributes** | Array of FiAttribute | Array of FI-specific attributes |
| **links** | Array of HateoasLink | Array of Image Identifiers (unique to Transaction) used to retrieve Images of check or transaction receipt

## 14.44 Transactions Entity

Array of transactions, implements PaginatedArray

| **Field** | **Type** | **Description** |
|---|---|---|
| **transactions** | zero or more of DepositTransaction, InvestmentTransaction, LoanTransaction, or LocTransaction | An array of transactions with entity types dependent on the account type (deposit, investment, loan, or line of credit) |

## 14.45 Transfer Entity

| **Field** | **Type** | **Description** |
|---|---|---|
| **transferId** | Identifier | Client generated, long-term persistent identity of the transfer action. This ID should be maintained and returned by institution. |
| **fromAccountId**  | Identifier | Long-term persistent identity of the source account |
| **toAccountId** | Identifier | Long-term persistent identity of the destination account |
| **amount** | Number | Positive amount of money to be transferred |
| **memo** | String255 | User-entered reason for transfer |
| **paymentDetails** | PaymentDetails | Payment details |

## 14.46 TransferStatus Entity

| **Field** | **Type** | **Description** |
|---|---|---|
| **transferId** | Identifier | Client generated, long-term persistent identity of the transfer action. This ID should be maintained and returned by institution. |
| **referenceId**  | Identifier | Long term persistent identifier for transfer attempt |
| **status** | TransferStatusStatus | SUCCESS, NOFUNDS, PENDING, FAILURE |
| **transferDate** | Timestamp | Date of transfer attempt |

## 14.47 Vesting Entity

| **Field**               | **Type**  | **Description**       |
|-------------------------|-----------|-----------------------|
| **vestingDate**         | Timestamp | Vesting date          |
| **symbol**              | string    | Security symbol       |
| **strikePrice**         | Number    | Strike price          |
| **vestingPercentage**   | Number    | Vesting percentage    |
| **otherVestAmount**     | Number    | Other vest amount     |
| **otherVestPercentage** | Number    | Other vest percentage |
| **vestedBalance**       | Number    | Vested balance        |
| **unVestedBalance**     | Number    | Unvested balance      |
| **vestedQuantity**      | Number    | Vested quantity       |
| **unVestedQuantity**    | Number    | Unvested quantity     |

## 14.48 InsuranceAccount Entity

Inherits and extends Account.

| **Field** | **Type** | **Description** |
|---|---|---|
| **accountCategory** | enum (DEPOSIT_ACCOUNT, INVESTMENT_ACCOUNT, LOAN_ACCOUNT, LOC_ACCOUNT, INSURANCE_ACCOUNT) | |
| **policyPremium** | Number | The amount of the user's premium |
| **policyPremiumTerm** | enum (MONTHLY, ANNUAL) | |
| **policyStartDate** | Timestamp | |
| **policyEndDate** | Timestamp | |
| **policyCoverageAmount** | Number | Total amount of money the user is insured for |
| **transactions** | Array of InsuranceTransaction | Array of Insurance Transactions |
| **bills** | Bills | |

## 14.49 InsuranceTransaction Entity

An Insurance transaction, inherits and extends Transaction

| **Field** | **Type** | **Description** |
|---|---|---|
| **transactionType** | enum (PAYMENT, FEE, ADJUSTMENT, INTEREST) | |

## 14.50 Bills Entity

| **Field** | **Type**  | **Description** |
|---|---|---|
| **totalPaymentDue** | number | Total payment due or next payment due. Monthly payment due for loans. |
| **minimumPaymentDue** | number | |
| **dueDate** | Timestamp | The date that the payment is due |
| **autoPayEnabled** | boolean | Whether the user's bill is paid automatically |
| **autoPayAmount** | number | The amount of money the user has set to autopay this bill |
| **autoPayDate** | Timestamp | The date the autopayment is set to trigger for this bill |
| **pastDueAmount** | number | The amount that the user should have already paid. Negative value if user owes money. |
| **lastPaymentAmount** | number | |
| **lastPaymentDate** | Timestamp | |
| **statementBalance**  | number | The amount of the last statement. Negative value if user owes money. |
| **statementDate** | Timestamp | The date the statement was issued |

## 14.51 PensionSource Entity

| **Field** | **Type** | **Description** |
|---|---|---|
| **displayName**   | string | Name of the Source |
| **amount** | number | Benefit Amount |
| **paymentOption** | string | Form of payment |
| **asOfDate** | Timestamp | Date benefit was calculated |
| **frequency** | PaymentFrequency | Frequency of Payment |
| **startDate** | Timestamp | Assumed retirement date - As of date amount is payable |

## 14.52 AnnuityAccount Entity

| **Field** | **Type** | **Description** |
|---|---|---|
| **annuityProductType** | AnnuityProductType | Deferred, Immediate |
| **annuityValueBasis** | AnnuityValueBasis  | Fixed, Variable |
| **paymentAmount** | Number | Amount of the recurring payment. |
| **paymentFrequency** | PaymentFrequency | ANNUALLY, QUARTERLY, MONTHLY, WEEKLY |
| **paymentStartDate** | TimeStamp | Date of first payment; could be a future date |
| **paymentEndDate** | TimeStamp | Date last payment will be made |
| **totalPaymentCount** | Number | Total number of payments that will be produced by the annuity |
| **netPresentValue** | Number | Surrender or cash balance value |
| **annualIncrease** | Number | Percent or dollar amount of annual payment increase |
| **annualIncreaseType** | AnnualIncreaseType | Fixed, Percent, Dollar |
| **periodCertainGuarantee** | PeriodCertainGuarantee | No Period Certain, 5-Year, 10-Year, 20-Year, 30-year |

## 14.53 HateoasLink

| **Field** | **Type** | **Description** |
|---|---|---|
| **href**<br>*required* |string | example: "/example/accounts/12345" |
| **action** | string | HTTP Method to use for the request<br>enum: [GET, POST, PATCH, DELETE, PUT] |
| **types** | ContentTypes | ContentTypes that can be used in the Accept header. |

## 14.54 PageMetadataLinks

| **Field** | **Type** | **Description** |
|---|---|---|
| **next** | HateoasLink | |
| **prev** | HateoasLink | |

## 14.55 PageMetadata

| **Field** | **Type** | **Description** |
|---|---|---|
| **nextOffset** | string | Opaque identifier. Does not need to be numeric or have any specific pattern. Implementation specific.<br>example: "2" |
| **prevOffset** | string | Opaque identifier. Does not need to be numeric or have any specific pattern. Implementation specific.<br>example: "2" |
| **totalElements** | integer | Total number of elements<br>example: 3 |

## 14.56 PaginatedArray

To be used as a mixin using "allOf"

| **Field** | **Type** | **Description** |
|---|---|---|
| **page** | PageMetadata| |
| **links** | PageMetadataLinks| |

## 14.57 Tax1041K1

Beneficiary's Share of Income, Deductions, Credits, etc.  Extends and inherits all fields from Tax.

| **Field** | **Type** | **Description** |
|---|---|---|
| **finalK1** | Boolean | Final K-1 |
| **amendedK1** | Boolean | Amended K-1 |
| **fiscalYearBegin** | Timestamp | Fiscal year begin date |
| **fiscalYearEnd** | Tiimestamp | Fiscal year end date |
| **trustTin** | string | Box A, Estate's or trust's employer identification number |
| **trustName** | string | Box B, Estate's or trust's name |
| **form1041T** | Boolean | Box D, Check if Form 1041-T was filed |
| **date1041T** | Timestamp | Box D, and enter the date it was filed |
| **final1041** | Boolean | Box E, Check if this is the final Form 1041 for the estate or trust |
| **beneficiaryTin** | string | Box F, Beneficiary's identifying number |
| **domestic** | Boolean | Box H, Domestic beneficiary |
| **foreign** | Boolean | Box H, Foreign beneficiary |
| **interestIncome** | Number | Box 1, Interest income |
| **ordinaryDividends** | Number | Box 2a, Ordinary dividends |
| **qualifiedDividends** | Number | Box 2b, Qualified dividends |
| **netShortTermGain** | Number | Box 3, Net short-term capital gain |
| **netLongTermGain** | Number | Box 4a, Net long-term capital gain |
| **gain28Rate** | Number | Box 4b, 28% rate gain |
| **unrecaptured1250Gain** | Number | Box 4c, Unrecaptured section 1250 gain |
| **otherPortfolioIncome** | Number | Box 5, Other portfolio and nonbusiness income |
| **ordinaryBusinessIncome** | Number | Box 6, Ordinary business income |
| **netRentalRealEstateIncome** | Number | Box 7, Net rental real estate income |
| **otherRentalIncome** | Number | Box 8, Other rental income |
| **directlyApportionedDeductions** | Array of CodeAmount | Box 9, Directly apportioned deductions |
| **estateTaxDeduction** | Number | Box 10, Estate tax deduction |
| **finalYearDeductions** | Array of CodeAmount | Box 11, Final year deductions |
| **fiduciaryNameAddress** | NameAddress | Box C, Fiduciary's name and address |
| **amtAdjustments** | Array of CodeAmount | Box 12, Alternative minimum tax adjustment |
| **beneficiaryNameAddress** | NameAddress | Box G, Beneficiary's name and address |
| **credits** | Array of CodeAmount | Box 13, Credits and credit recapture |
| **otherInfo** | Array of CodeAmount | Box 14, Other information |

## 14.58 Tax1065K1

Partner's Share of Income, Deductions, Credits, etc.  Extends and inherits all fields from Tax.

| **Field** | **Type** | **Description** |
|---|---|---|
| **fiscalYearBegin** | Timestamp | Fiscal year begin date |
| **fiscalYearEnd** | Timestamp | Fiscal year end date |
| **finalK1** | Boolean | Final K-1 |
| **amendedK1** | Boolean | Amended K-1 |
| **partnershipTin** | string | Box A, Partnership's employer identification number |
| **partnershipNameAddress** | NameAddress | Box B, Partnership's name, address, city, state, and ZIP code |
| **irsCenter** | string | Box C, IRS Center where partnership filed return |
| **publiclyTraded** | Boolean | Box D, Check if this is a publicly traded partnership (PTP) |
| **partnerTin** | string | Box E, Partner's identifying number |
| **partnerNameAddress** | NameAddress | Box F, Partner's name, address, city, state, and ZIP code |
| **generalPartner** | Boolean | Box G, General partner or LLC member-manager |
| **limitedPartner** | Boolean | Box G, Limited partner or other LLC member |
| **domestic** | Boolean | Box H1, Domestic partner |
| **foreign** | Boolean | Box H1, Foreign partner |
| **disregardedEntity** | Boolean | Box H2, Check if the partner is a disregarded entity (DE), and enter the partner's TIN and Name |
| **disregardedEntityTin** | string | Box H2, Disregarded entity partner's TIN |
| **disregardedEntityName** | string | Box H2, Disregarded entity partner's Name |
| **entityType** | string | Box I1, What type of entity is this partner? |
| **retirementPlan** | Boolean | Box I2, If this partner is a retirement plan (IRA/SEP/Keogh/etc.), check here |
| **profitShareBegin** | Number | Box J, Partner's share of profit - beginning |
| **profitShareEnd** | Number | Box J, Partner's share of profit - ending |
| **lossShareBegin** | Number | Box J, Partner's share of loss - beginning |
| **lossShareEnd** | Number | Box J, Partner's share of loss - ending |
| **capitalShareBegin** | Number | Box J, Partner's share of capital - beginning |
| **capitalShareEnd** | Number | Box J, Partner's share of capital - ending |
| **nonrecourseLiabilityShareBegin** | Number | Box K, Partner's share of liabilities - beginning - nonrecourse |
| **nonrecourseLiabilityShareEnd** | Number | Box K, Partner's share of liabilities - ending - nonrecourse |
| **qualifiedLiabilityShareBegin** | Number | Box K, Partner's share of liabilities - beginning - qualified nonrecourse financing |
| **qualifiedLiabilityShareEnd** | Number | Box K, Partner's share of liabilities - ending - qualified nonrecourse financing |
| **recourseLiabilityShareBegin** | Number | Box K, Partner's share of liabilities - beginning - recourse |
| **recourseLiabilityShareEnd** | Number | Box K, Partner's share of liabilities - ending - recourse |
| **includesLowerTierLiability** | Boolean | Box K, Check this box if item K includes liability amounts from lower tier partnerships |
| **capitalAccountBegin** | Number | Box L, Partner's capital account analysis - Beginning capital account |
| **capitalAccountContributions** | Number | Box L, Partner's capital account analysis - Capital contributed during the year |
| **capitalAccountIncrease** | Number | Box L, Partner's capital account analysis - Current year net income (loss) |
| **capitalAccountOther** | Number | Box L, Partner's capital account analysis - Other increase (decrease) |
| **capitalAccountWithdrawals** | Number | Box L, Partner's capital account analysis - Withdrawals & distributions |
| **capitalAccountEnd** | Number | Box L, Partner's capital account analysis - Ending capital account |
| **bookTax** | Boolean | Box L, Tax basis (IRS removed 2019) |
| **bookGaap** | Boolean | Box L, GAAP (IRS removed 2019) |
| **book704b** | Boolean | Box L, Section 704(b) book (IRS removed 2019) |
| **bookOther** | Boolean | Box L, Other (explain) (IRS removed 2019) |
| **bookOtherExplain** | string | Box L, Other (explain) (IRS removed 2019) |
| **builtInGain** | Boolean | Box M, Did the partner contribute property with a built-in gain or loss? - Yes |
| **unrecognizedSection704Begin** | Boolean | Box N, Partner's Share of Net Unrecognized Section 704(c) Gain or (Loss) - beginning |
| **unrecognizedSection704End** | Boolean | Box N, Partner's Share of Net Unrecognized Section 704(c) Gain or (Loss) - ending |
| **ordinaryIncome** | Number | Box 1, Ordinary business income (loss) |
| **netRentalRealEstateIncome** | Number | Box 2, Net rental real estate income (loss) |
| **otherRentalIncome** | Number | Box 3, Other net rental income (loss) |
| **guaranteedPaymentServices** | Number | Box 4a, Guaranteed payments for services |
| **guaranteedPaymentCapital** | Number | Box 4b, Guaranteed payments for capital |
| **guaranteedPayment** | Number | Box 4c, Total guaranteed payments |
| **interestIncome** | Number | Box 5, Interest income |
| **ordinaryDividends** | Number | Box 6a, Ordinary dividends |
| **qualifiedDividends** | Number | Box 6b, Qualified dividends |
| **dividendEquivalents** | Number | Box 6c, Dividend equivalents |
| **royalties** | Number | Box 7, Royalties |
| **netShortTermGain** | Number | Box 8, Net short-term capital gain (loss) |
| **netLongTermGain** | Number | Box 9a, Net long-term capital gain (loss) |
| **collectiblesGain** | Number | Box 9b, Collectibles (28%) gain (loss) |
| **unrecaptured1250Gain** | Number | Box 9c, Unrecaptured section 1250 gain |
| **net1231Gain** | Number | Box 10, Net section 1231 gain (loss) |
| **otherIncome** | Array of CodeAmount | Box 11, Other income |
| **section179Deduction** | Number | Box 12, Section 179 deduction |
| **otherDeductions** | Array of CodeAmount | Box 13, Other deductions |
| **selfEmployment** | Array of CodeAmount | Box 14, Self-employment earnings (loss) |
| **credits** | Array of CodeAmount | Box 15, Credits |
| **foreignCountry** | string | Box 16, Foreign country |
| **foreignTransactions** | Array of CodeAmount | Box 16, Foreign transactions |
| **amtItems** | Array of CodeAmount | Box 17, Alternative minimum tax (AMT) items |
| **taxExemptIncome** | Array of CodeAmount | Box 18, Tax-exempt income and nondeductible expenses |
| **distributions** | Array of CodeAmount | Box 19, Distributions |
| **otherInfo** | Array of CodeAmount | Box 20, Other information |
| **multipleAtRiskActivities** | Boolean | Box 21, More than one activity for at-risk purposes |
| **multiplePassiveActivities** | Boolean | Box 22, More than one activity for passive activity purposes |

## 14.59 Tax1095A

Health Insurance Marketplace Statement.  Extends and inherits all fields from Tax.

| **Field** | **Type** | **Description** |
|---|---|---|
| **marketplaceId** | string | Box 1, Marketplace identifier |
| **marketplacePolicyNumber** | string | Box 2, Marketplace-assigned policy number |
| **policyIssuerName** | string | Box 3, Policy issuer's name |
| **recipientName** | string | Box 4, Recipient's name |
| **recipientTin** | string | Box 5, Recipient's SSN |
| **recipientDateOfBirth** | Timestamp | Box 6, Recipient's date of birth |
| **spouseName** | string | Box 7, Recipient's spouse's name |
| **spouseTin** | string | Box 8, Recipient's spouse's SSN |
| **spouseDateOfBirth** | Timestamp | Box 9, Recipient's spouse's date of birth |
| **policyStartDate** | Timestamp | Box 10, Policy start date |
| **policyTerminationDate** | Timestamp | Box 11, Policy termination date |
| **recipientAddress** | Address | Boxes 12-15, Recipient address |
| **coveredIndividuals** | Array of HealthInsuranceMarketplaceCoveredIndividual | Boxes 16+, Covered Individuals |
| **coverages** | Array of HealthInsuranceCoverage | Boxes 21-33, Coverage Information |

## 14.60 Tax1095B

Health Coverage.  Extends and inherits all fields from Tax.

| **Field** | **Type** | **Description** |
|---|---|---|
| **responsibleName** | IndividalName | Box 1, Name of responsible individual |
| **responsibleTin** | string | Box 2, Social security number (SSN or other TIN) |
| **responsibleDateOfBirth** | Timestamp | Box 3, Date of birth (if SSN or other TIN is not available) |
| **responsibleAddress** | Address | Boxes 4-7, Address of responsible individual |
| **originOfHealthCoverageCode** | string | Box 8, Enter letter identifying Origin of the Health Coverage |
| **employerNameAddress** | NameAddress | Boxes 10, 12-15, Employer name and address |
| **employeId** | string | Box 11, Employer identification number (EIN) |
| **issuerNameAddressPhone** | NameAddressPhone | Boxes 16, 18-22, Issuer name, address, and phone |
| **issuerId** | string | Box 17, Employer identification number (EIN) |
| **coveredIndividuals** | Array of HealthInsuranceCoveredIndividual | Boxes 23+, Covered Individuals |

## 14.61 Tax1095C

Employer-Provided Health Insurance Offer and Coverage.  Extends and inherits all fields from Tax.

| **Field** | **Type** | **Description** |
|---|---|---|
| **employeeName** | IndividualName | Box 1, Employee name |
| **tin** | string | Box 2, Social security number (SSN) |
| **employeeAddress** | Address | Boxes 3-6, Employee address |
| **employerNameAddressPhone** | NameAddressPhone | Boxes 7, 9-13, Employer name, address and phone |
| **employerId** | string | Box 8, Employer identification number (EIN) |
| **selfInsuredCoverage** | Boolean | Self Insured Coverage |
| **offersOfCoverage** | Array of OfferOfHealthInsuranceCoverage | Boxes 14-16, Employee Offer of Coverage |
| **planStartMonth** | integer | Plan Start Month |
| **coveredIndividuals** | Array of HealthInsuranceCoveredIndividual | Boxes 17+, Covered Individuals |

## 14.62 Tax1097Btc

Bond Tax Credit.  Extends and inherits all fields from Tax.

| **Field** | **Type** | **Description** |
|---|---|---|
| **issuerNameAddress** | NameAddressPhone | Issuer's name, address, and phone |
| **issuerTin** | string | FORM 1097-BTC ISSUER'S TIN |
| **recipientTin** | string | RECIPIENT'S TIN |
| **recipientNameAddress** | NameAddress | Recipient's name and address |
| **filingForCredit** | Boolean | Form 1097-BTC issuer is: Issuer of bond or its agent filing 2019 Form 1097-BTC for credit being reported |
| **asNominee** | Boolean | Form 1097-BTC issuer is: An entity or a person that received or should have received a 2019 Form 1097-BTC and is distributing part or all of that credit to others |
| **total** | Number | Box 1, Total |
| **bondCode** | string | Box 2a, Code |
| **uniqueId** | string | Box 2b, Unique Identifier |
| **bondType** | string | Box 3, Bond type |
| **amounts** | Array of MonthAmount | Box 5, Amounts by month |
| **comments** | string | Box 6, Comments |

## 14.63 Tax1098

Mortgage Interest Statement.  Extends and inherits all fields from Tax.

| **Field** | **Type** | **Description** |
|---|---|---|
| **lenderNameAddress** | NameAddressPhone | Lender's name, address and phone |
| **lenderTin** | string | RECIPIENT'S/LENDER'S TIN |
| **borrowerTin** | string | PAYER'S/BORROWER'S TIN |
| **borrowerNameAddress** | NameAddress | Borrower's name and address |
| **mortgagedProperties** | integer | Box 9, Number of properties securing the mortgage |
| **otherInformation** | string | Box 10, Other (property tax) |
| **accountNumber** | string | Account number |
| **mortgageInterest** | Number | Box 1, Mortgage interest received from borrower |
| **outstandingPrincipal** | Number | Box 2, Outstanding mortgage principal |
| **originationDate** | Timestamp | Box 3, Mortgage origination date |
| **overpaidRefund** | Number | Box 4, Refund of overpaid interest |
| **mortgageInsurance** | Number | Box 5, Mortgage insurance premiums |
| **pointsPaid** | Number | Box 6, Points paid on purchase of principal residence |
| **isPropertyAddressSameAsBorrowerAddress** | Boolean | Box 7, Is address of property securing mortgage same as PAYER'S/BORROWER'S address |
| **acquisitionDate** | Timestamp | Box 11, Mortgage acquisition date |
| **propertyAddress** | Address | Box 8, Address of property securing mortgage |
| **propertyTax** | Number | Box 10, Property tax |
| **propertyDescription** | string | Box 8, Description of property securing mortgage, if property securing mortgage has no address |

## 14.64 Tax1098C

Contributions of Motor Vehicles, Boats, and Airplanes.  Extends and inherits all fields from Tax.

| **Field** | **Type** | **Description** |
|---|---|---|
| **doneeNameAddress** | NameAddressPhone | Donee's name, address and phone |
| **doneeTin** | string | DONEE'S TIN |
| **donorTin** | string | DONOR'S TIN |
| **donorNameAddress** | NameAddress | Donor's name and address |
| **dateOfContribution** | Timestamp | Box 1, Date of contribution |
| **odometerMileage** | integer | Box 2a, Odometer mileage |
| **carYear** | integer | Box 2b, Year |
| **make** | string | Box 2c, Make |
| **model** | string | Box 2d, Model |
| **vin** | string | Box 3, Vehicle or other identification number |
| **armsLengthTransaction** | Boolean | Box 4a, Donee certifies that vehicle was sold in arm's length transaction to unrelated party |
| **dateOfSale** | Timestamp | Box 4b, Date of sale |
| **grossProceeds** | Number | Box 4c, Gross proceeds from sale (see instructions) |
| **notTransferredBefore** | Boolean | Box 5a, Donee certifies that vehicle will not be transferred for money, other property, or services before completion of material improvements or significant intervening use |
| **needyIndividual** | Boolean | Box 5b, Donee certifies that vehicle is to be transferred to a needy individual for significantly below fair market value in furtherance of donee's charitable purpose |
| **descriptionOfImprovements** | string | Box 5c, Donee certifies the following detailed description of material improvements or significant intervening use and duration of use |
| **goodsInExchange** | Boolean | Box 6a, Did you provide goods or services in exchange for the vehicle? Yes |
| **valueOfExchange** | Number | Box 6b, Value of goods and services provided in exchange for the vehicle |
| **intangibleReligious** | Boolean | Box 6c, If this box is checked, donee certifies that the goods and services consisted solely of intangible religious benefits |
| **descriptionOfGoods** | string | Box 6c, Describe the goods and services, if any, that were provided. |
| **maxDeductionApplies** | Boolean | Box 7, Under the law, the donor may not claim a deduction of more than $500 for this vehicle if this box is checked |

## 14.65 Tax1098E

Student Loan Interest Statement.  Extends and inherits all fields from Tax.

| **Field** | **Type** | **Description** |
|---|---|---|
| **lenderNameAddress** | NameAddressPhone | Lender's name, address, and phone |
| **lenderTin** | string | LENDER'S TIN |
| **borrowerTin** | string | BORROWER'S TIN |
| **borrowerNameAddress** | NameAddress | Borrower's name and address |
| **accountNumber** | string | Account number |
| **studentLoanInterest** | Number | Box 1, Student loan interest received by lender |
| **box1ExcludesFees** | Boolean | ox 2, If checked, box 1 does not include loan origination fee made before September 1, 2004 |

## 14.66 Tax1098T

Tuition Statement.  Extends and inherits all fields from Tax.

| **Field** | **Type** | **Description** |
|---|---|---|
| **filerNameAddress** | NameAddressPhone | Filer's name, address, and phone |
| **filerTin** | string | Filer's federal identification number |
| **studentTin** | string | Student's social security number |
| **studentNameAddress** | NameAddress | Student's name and address |
| **accountNumber** | string | Account number |
| **halfTime** | Boolean | Box 8, Check if at least half-time student |
| **qualifiedTuitionFees** | Number | Box 1, Payments received for qualified tuition and related expenses |
| **adjustmentPriorYear** | Number | Box 4, Adjustments made for a prior year |
| **scholarship** | Number | Box 5, Scholarships or grants |
| **adjustScholarship** | Number | Box 6, Adjustments to scholarships or grants for a prior year |
| **includeJanMar** | Boolean | Box 7, Check if the amount in box 1 or box 2 includes amounts for an academic period beginning January - March 2018 |
| **graduate** | Boolean | Box 9, Check if graduate student |
| **insuranceRefund** | Number | Box 10, Insurance contract reimbursement / refund |

## 14.67 Tax1099A

Acquisition or Abandonment of Secured Property.  Extends and inherits all fields from Tax.

| **Field** | **Type** | **Description** |
|---|---|---|
| **lenderNameAddress** | NameAddressPhone | Lender's name, address, and phone |
| **lenderTin** | string | LENDER'S TIN |
| **borrowerTin** | string | BORROWER'S TIN |
| **borrowerNameAddress** | NameAddress | Borrower's name and address |
| **accountNumber** | string | Account number |
| **dateOfAcquisition** | Timestamp | Box 1, Date of lender's acquisition or knowledge of abandonment |
| **principalBalance** | Number | Box 2, Balance of principal outstanding |
| **fairMarketValue** | Number | Box 4, Fair market value property |
| **personallyLiable** | Boolean | Box 5, If checked, the borrower was personally liable for repayment of the debt |
| **propertyDescription** | string | Box 6, Description of property |

## 14.68 Tax1099B

Proceeds from Broker and Barter Exchange Transactions.  Extends and inherits all fields from Tax.

| **Field** | **Type** | **Description** |
|---|---|---|
| **payerNameAddress** | NameAddressPhone | Payer's name, address, and phone |
| **payerTin** | string | Payer's federal identification number |
| **recipientTin** | string | Recipient's identification number |
| **recipientNameAddress** | NameAddress | Recipient's name and address |
| **accountNumber** | string | Account number |
| **stateTaxWithholding** | StateTaxWithholding | Boxes 14-16, State tax withholding |
| **federalTaxWithheld** | Number | Box 4, Federal income tax withheld |
| **profitOnClosedContracts** | Number | Box 8, Profit or (loss) realized in 2019 on closed contracts |
| **unrealizedProfitOpenContractsBegin** | Number | Box 9, Unrealized profit or loss on open contracts - 12/31/2018 |
| **unrealizedProfitOpenContractsEnd** | Number | Box 10, Unrealized profit or loss on open contracts - 12/31/2019 |
| **aggregateProfitOnContracts** | Number | Box 11, Aggregate profit or (loss) on contracts |
| **bartering** | Number | Box 13, Bartering |
| **securityDetails** | Array of SecurityDetail | Boxes 1-3, 5-7, 12, Security details |

## 14.69 Tax1099C

Cancellation of Debt.  Extends and inherits all fields from Tax.

| **Field** | **Type** | **Description** |
|---|---|---|
| **creditorNameAddress** | NameAddressPhone | Creditor's name, address, and phone |
| **creditorTin** | string | CREDITOR'S TIN |
| **debtorTin** | string | DEBTOR'S TIN |
| **debtorNameAddress** | NameAddress | Debtor's name and address |
| **accountNumber** | string | Account number |
| **dateOfEvent** | Timestamp | Box 1, Date of identifiable event |
| **amountDischarged** | Number | Box 2, Amount of debt discharged |
| **interestIncluded** | Number | Box 3, Interest if included in box 2 |
| **debtDescription** | Number | Box 4, Debt description |
| **personallyLiable** | Boolean | Box 5, If checked, the debtor was personally liable for repayment of the debt |
| **debtCode** | string | Box 6, Identifiable debt code |
| **fairMarketValue** | Number | Box 7, Fair market value of property |

## 14.70 Tax1099Cap

Changes in Corporate Control and Capital Structure.  Extends and inherits all fields from Tax.

| **Field** | **Type** | **Description** |
|---|---|---|
| **corporationNameAddress** | NameAddressPhone | Corporation's name, address, and phone |
| **corporationTin** | string | CORPORATION'S TIN |
| **shareholderTin** | string | SHAREHOLDER'S TIN |
| **shareholderNameAddress** | NameAddress | Shareholder's name and address |
| **accountNumber** | string | Account number |
| **dateOfSale** | Timestamp | Box 1, Date of sale or exchange |
| **aggregateAmount** | Number | Box 2, Aggregate amount received |
| **numberOfShares** | Number | Box 3, Number of shares exchanged |
| **stockClasses** | string | Box 4, Classes of stock exchanged |

## 14.71 Tax1099Div

Dividends and Distributions.  Extends and inherits all fields from Tax.

| **Field** | **Type** | **Description** |
|---|---|---|
| **payerNameAddress** | NameAddressPhone | Payer's name, address and phone |
| **payerTin** | string | Payer's federal identification number |
| **recipientTin** | string | Recipient's identification number |
| **recipientNameAddress** | NameAddress | Recipient's name and address |
| **foreignAccountTaxCompliance** | Boolean | FATCA filing requirement |
| **accountNumber** | string | Account number |
| **ordinaryDividends** | Number | Box 1a, Total ordinary dividends |
| **qualifiedDividends** | Number | Box 1b, Qualified dividends |
| **totalCapitalGain** | Number | Box 2a, Total capital gain distributions |
| **unrecaptured1250Gain** | Number | Box 2b, Unrecaptured Section 1250 gain |
| **section1202Gain** | Number | Box 2c, Section 1202 gain |
| **collectiblesGain** | Number | Box 2d, Collectibles (28%) gain |
| **nonTaxableDistribution** | Number | Box 3, Nondividend distributions |
| **federalTaxWithheld** | Number | Box 4, Federal income tax withheld |
| **section199ADividends** | Number | Box 5, Section 199A dividends |
| **investmentExpenses** | Number | Box 6, Investment expenses |
| **foreignTaxPaid** | Number | Box 7, Foreign tax paid |
| **foreignCountry** | string | Box 8, Foreign country or U.S. possession |
| **cashLiquidation** | Number | Box 9, Cash liquidation distributions |
| **nonCashLiquidation** | Number | Box 10, Noncash liquidation distributions |
| **taxExemptInterestDividend** | Number | Box 11, Exempt-interest dividends |
| **specifiedPabInterestDividend** | Number | Box 12, Specified private activity bond interest dividends |
| **stateTaxWithholding** | StateTaxWithholding | Boxes 13-15, State tax withholding |
| **foreignIncomes** | Array of DescriptionAmount | Supplemental: Foreign income information |
| **stateTaxExemptIncomes** | Array of DescriptionAmount | Supplemental: Tax exempt income state information |

## 14.72 Tax1099G

Certain Government Payments. Extends and inherits all fields from Tax

| **Field** | **Type** | **Description** |
|---|---|---|
| **payerNameAddress**	| NameAddressPhone	| Payer's name, address, and phone	|
| **payerTin**	| string	| PAYER'S TIN	|
| **recipientTin**	| string	| RECIPIENT'S TIN	|
| **recipientNameAddress**	| NameAddress	| Recipient's name and address	|
| **accountNumber**	| string	| Account number	|
| **unemploymentCompensation**	| number (double)	| Box 1, Unemployment compensation	|
| **taxRefund**| number (double)	| Box 2, State or local income tax refunds, credits, or offsets	|
| **refundYear**	| integer (int32)	| Box 3, Box 2 amount is for tax year	|
| **federalTaxWithheld**	| number (double)	| Box 4, Federal income tax withheld	|
| **rtaaPayments**	| number (double)	| Box 5, RTAA payments	|
| **grants**	| number (double)	| Box 6, Taxable grants	|
| **agriculturePayments**	| number (double)	| Box 7, Agriculture payments	|
| **businessIncome**	| Boolean	| Box 8, If checked, box 2 is trade or business income	|
| **marketGain**	| number (double)	| Box 9, Market gain	|
| **stateTaxWithholding**	| array of StateTaxWithholding	| Boxes 10-11, State tax withholding	|

## 14.73 Tax1099H

Health Coverage Tax Credit (HCTC) Advance Payments. Extends and inherits all fields from Tax

| **Field** | **Type** | **Description** |
|---|---|---|
| **issuerNameAddress**	| NameAddressPhone	| Issuer's name, address, and phone	|
| **issuerTin**	| string	| ISSUER'S/PROVIDER'S federal identification number	|
| **recipientTin**	| string	| RECIPIENT'S identification number	|
| **recipientNameAddress**	| NameAddress	| Recipient's name and address	|
| **advancePayments**	| number (double)	| Box 1, Amount of HCTC advance payments	|
| **numberOfMonths**	| integer (int32)	| Box 2, Number of months HCTC advance payments and reimbursement credits paid to you	|
| **payments**	| array of MonthAmount	| Boxes 3-14, Payments by month	|

## 14.74 Tax1099Int

Interest Income. Extends and inherits all fields from Tax.

| **Field** | **Type** | **Description** |
|---|---|---|
| **payerNameAddress**	| NameAddressPhone	| Payer's name, address, and phone	|
| **payerTin**	| string	| Payer's TIN	|
| **recipientTin**	| string	| Recipient's TIN	|
| **recipientNameAddress**	| NameAddress	| Recipient's name and address	|
| **foreignAccountTaxCompliance**	| Boolean	| FATCA filing requirement	|
| **accountNumber**	| string	| Account number	|
| **payerRtn**	| string	| Payer's RTN	|
| **interestIncome**	| number (double)	| Box 1, Interest income	|
| **earlyWithdrawalPenalty**	| number (double)	| Box 2, Early withdrawal penalty	|
| **usBondInterest**	| number (double)	| Box 3, Interest on U.S. Savings Bonds and Treasury obligations	|
| **federalTaxWithheld**	| number (double)	| Box 4, Federal income tax withheld	|
| **investmentExpenses**	| number (double)	| Box 5, Investment expenses	|
| **foreignTaxPaid**	| number (double)	| Box 6, Foreign tax paid	|
| **foreignCountry**	| string	| Box 7, Foreign country or U.S. possession	|
| **taxExemptInterest**	| number (double)	| Box 8, Tax-exempt interest	|
| **specifiedPabInterest**	| number (double)	| Box 9, Specified private activity bond interest	|
| **marketDiscount**	| number (double)	| Box 10, Market discount	|
| **bondPremium**	| number (double)	| Box 11, Bond premium	|
| **usBondPremium**	| number (double)	| Box 12, Bond premium on Treasury obligations	|
| **taxExemptBondPremium**	| number (double)	| Box 13, Bond premium on tax-exempt bond	|
| **cusipNumber**	| string	| Box 14, Tax-exempt bond CUSIP no.	|
| **stateTaxWithholding**	| array of StateTaxWithholding	| Boxes 15-17, State tax withholding	|
| **foreignIncomes**	| array of DescriptionAmount	| Supplemental: Foreign income amounts	|
| **stateTaxExemptIncome**	| array of DescriptionAmount	| Supplemental: Tax-exempt income by state	|

## 14.75 Tax1099K

Merchant Card and Third-Party Network Payments. Extends and inherits all fields from Tax

| **Field** | **Type** | **Description** |
|---|---|---|
| **filerNameAddress**	| NameAddressPhone	| Filer's name, address, and phone	|
| **paymentSettlementEntity**	| Boolean	| Check to indicate if FILER is a Payment settlement entity (PSE)	|
| **electronicPaymentFacilitator**	| Boolean	| Check to indicate if FILER is an Electronic Payment Facilitator (EPF)/Other third party	|
| **paymentCard**	| Boolean	| Check to indicate transactions reported are: Payment card	|
| **thirdPartyNetwork**	| Boolean	| Check to indicate transactions reported are: Third party network	|
| **payeeNameAddress**	| NameAddress	| Payee's name and address	|
| **pseName**	| string	| PSE's name	|
| **accountNumber**	| string	| Account number	|
| **filerTin**	| string	| FILER'S TIN	|
| **payeeFederalId**	| string	| PAYEE'S taxpayer identification no.	|
| **grossAmount**	| number (double)	| Box 1a, Gross amount of payment card/third party network transactions	|
| **cardNotPresent**	| number (double)	| Box 1b, Card Not Present Transactions	|
| **merchantCategoryCode**	| string	| Box 2, Merchant category code	|
| **numberOfTransactions**	| number (double)	| Box 3, Number of purchase transactions	|
| **federalTaxWithheld**	| number (double)	| Box 4, Federal income tax withheld	|
| **monthAmounts**	| array of MonthAmount	| Box 5, Monthly amounts	|
| **stateTaxWithholding**	| array of StateTaxWithholding	| Boxes 6-8, State tax withholding	|
| **psePhone**	| TelephoneNumberPlusExtension	| PSE's phone number	|

## 14.76 Tax1099Ltc

Long-Term Care and Accelerated Death Benefits. Extends and inherits all fields from Tax

| **Field** | **Type** | **Description** |
|---|---|---|
| **payerNameAddress**	| NameAddressPhone	| Payer's name, address, and phone	|
| **payerTin**	| string	| PAYER'S TIN	|
| **policyholderTin**	| string	| POLICYHOLDER'S TIN	|
| **policyHolderNameAddress**	| NameAddress	| Policyholder name and address	|
| **accountNumber**	| string	| Account number	|
| **ltcBenefits**	| number (double)	| Box 1, Gross long-term care benefits paid	|
| **deathBenefits**	| number (double)	| Box 2, Accelerated death benefits paid	|
| **perDiem**	| Boolean	| Box 3, Per diem	|
| **reimbursedAmount**	| Boolean	| Box 3, Reimbursed amount	|
| **insuredId**	| string	| INSURED'S taxpayer identification no.	|
| **insuredNameAddress**	| NameAddress	| Insured name and address	|
| **qualifiedContract**	| Boolean	| Box 4, Qualified contract	|
| **chronicallyIll**	| Boolean	| Box 5, Chronically ill	|
| **terminallyIll**	| Boolean	| Box 5, Terminally ill	|
| **dateCertified**	| Timestamp	| Date certified |

## 14.77 Tax1099Misc

Miscellaneous Income. Extends and inherits all fields from Tax.

| **Field** | **Type** | **Description** |
|---|---|---|
| **payerNameAddress**	| NameAddressPhone	| Payer's name, address, and phone	|
| **payerTin**	| string	| Payer's identification number	|
| **recipientTin**	| string	| Recipient's identification number	|
| **recipientNameAddress**	| NameAddress	| Recipient's name and address	|
| **accountNumber**	| string	| Account number	|
| **foreignAccountTaxCompliance**	| Boolean	| FATCA filing requirement	|
| **rents**	| number (double)	| Box 1, Rents	|
| **royalties**	| number (double)	| Box 2, Royalties	|
| **otherIncome**	| number (double)	| Box 3, Other income	|
| **federalTaxWithheld**	| number (double)	| Box 4, Federal income tax withheld	|
| **fishingBoatProceeds**	| number (double)	| Box 5, Fishing boat proceeds	|
| **medicalHealthPayment**	| number (double)	| Box 6, Medical and health care payments	|
| **nonEmployeeCompensation**	| number (double)	| Box 7, Nonemployee compensation	|
| **substitutePayments**	| number (double)	| Box 8, Substitute payments in lieu of dividends or interest	|
| **payerDirectSales**	| Boolean	| Box 9, Payer made direct sales of $5,000 or more	|
| **cropInsurance**	| number (double)	| Box 10, Crop insurance proceeds	|
| **excessGolden**	| number (double)	| Box 13, Excess golden parachute payments	|
| **grossAttorney**	| number (double)	| Box 14, Gross proceeds paid to an attorney	|
| **section409ADeferrals**	| number (double)	| Box 15a, Section 409A deferrals	|
| **section409AIncome**	| number (double)	| Box 15b, Section 409A income	|
| **stateTaxWithholding**	| array of StateTaxWithholding	| Boxes 16-18, State tax withholding	|

## 14.78 Tax1099Oid

Original Issue Discount. Extends and inherits all fields from Tax.

| **Field** | **Type** | **Description** |
|---|---|---|
| **payerNameAddress**	| NameAddressPhone	| Payer's name, address and phone	|
| **payerTin**	| string	| Payer's TIN	|
| **recipientTin**	| string	| Recipient's TIN	|
| **recipientNameAddress**	| NameAddress	| Recipient's name and address	|
| **foreignAccountTaxCompliance**	| Boolean	| FATCA filing requirement	|
| **accountNumber**	| string	| Account number	|
| **originalIssueDiscount**	| number (double)	| Box 1, Original issue discount	|
| **otherPeriodicInterest**	| number (double)	| Box 2, Other periodic interest	|
| **earlyWithdrawalPenalty**	| number (double)	| Box 3, Early withdrawal penalty	|
| **federalTaxWithheld**	| number (double)	| Box 4, Federal income tax withheld	|
| **marketDiscount**	| number (double)	| Box 5, Market discount	|
| **acquisitionPremium**	| number (double)	| Box 6, Acquisition premium	|
| **oidDescription**	| string	| Box 7, Description	|
| **discountOnTreasuryObligations**	| number (double)	| Box 8, Original issue discount on U.S. Treasury obligations	|
| **investmentExpenses**	| number (double)	| Box 9, Investment expenses	|
| **bondPremium**	| number (double)	| Box 10, Bond premium	|
| **taxExemptOid**	| number (double)	| Box 11, Tax-exempt OID	|
| **stateTaxWithholding**	| array of StateTaxWithholding	| Boxes 12-14, State tax withheld	|
| **stateExemptOid**	| array of DescriptionAmount	| Supplemental: State name and tax-exempt OID by state	|

## 14.79 Tax1099Patr

Taxable Distributions Received From Cooperatives. Extends and inherits all fields from Tax.

| **Field** | **Type** | **Description** |
|---|---|---|
| **payerNameAddress**	| NameAddressPhone	| Payer's name, address, and phone	|
| **payerTin**	| string	| PAYER'S TIN	|
| **recipientTin**	| string	| RECIPIENT'S TIN	|
| **recipientNameAddress**	| NameAddress	| Recipient's name and address	|
| **accountNumber**	| string	| Account number	|
| **patronageDividends**	| number (double)	| Box 1, Patronage dividends	|
| **nonpatronageDistributions**	| number (double)	| Box 2, Nonpatronage distributions	|
| **perUnitRetainAllocations**	| number (double)	| Box 3, Per-unit retain allocations	|
| **federalTaxWithheld**	| number (double)	| Box 4, Federal income tax withheld	|
| **redemption**	| number (double)	| Box 5, Redemption of nonqualified notices and retain allocations	|
| **dpaDeduction**	| number (double)	| Box 6, Domestic production activities deduction	|
| **qualifiedPayments**	| number (double)	| Box 7, Qualified payments	|
| **investmentCredit**	| number (double)	| Box 8, Investment credit	|
| **workOpportunityCredit**	| number (double)	| Box 9, Work opportunity credit	|
| **patronsAmtAdjustment**	| number (double)	| Box 10, Patron's AMT adjustment	|
| **otherCreditsAndDeductions**	| number (double)	| Box 11, Other credits and deductions	|

## 14.80 Tax1099Q

Payments from Qualified Education Programs. Extends and inherits all fields from Tax.

| **Field** | **Type** | **Description** |
|---|---|---|
| **payerNameAddress**	| NameAddressPhone	| Payer's name, address, and phone	|
| **payerTin**	| string	| PAYER'S/TRUSTEE'S TIN	|
| **recipientTin**	| string	| RECIPIENT'S TIN	|
| **recipientNameAddress**	| NameAddress	| Recipient's name and address	|
| **accountNumber**	| string	| Account number	|
| **grossDistribution**	| number (double)	| Box 1, Gross distribution	|
| **earnings**	| number (double)	| Box 2, Earnings	|
| **basis**	| number (double)	| Box 3, Basis	|
| **trusteeToTrustee**	| Boolean	| Box 4, Trustee-to-trustee transfer	|
| **tuitionPlanPrivate**	| Boolean	| Box 5a, Qualified tuition plan - Private	|
| **tuitionPlanPublic**	| Boolean	| Box 5b, Qualified tuition plan - Public	|
| **coverdellEsa**	| Boolean	| Box 5c, Coverdell ESA	|
| **recipientIsNotBeneficiary**	| Boolean	| Box 6, If this box is checked, the recipient is not the designated beneficiary	|

## 14.81 Tax1099R

Distributions from Pensions, Annuities, Retirement or Profit-Sharing Plans, IRAs, Insurance Contracts, etc. Extends and inherits all fields from Tax.

| **Field** | **Type** | **Description** |
|---|---|---|
| **payerNameAddress**	| NameAddressPhone	| Payer's name, address, and phone	|
| **payerTin**	| string	| PAYER'S TIN	|
| **recipientTin**	| string	| RECIPIENT'S TIN	|
| **recipientNameAddress**	| NameAddress	| Recipient's name and address	|
| **allocableToIRR**	| number (double)	| Box 10, Amount allocable to IRR within 5 years	|
| **firstYearOfRoth**	| integer (int32)	| Box 11, First year of designated Roth	|
| **foreignAccountTaxCompliance**	| Boolean	| FATCA filing requirement	|
| **recipientAccountNumber**	| string	| Account number	|
| **dateOfPayment**	| Timestamp	| Date of payment	|
| **grossDistribution**	| number (double)	| Box 1, Gross distribution	|
| **taxableAmount**	| number (double)	| Box 2a, Taxable amount	|
| **taxableAmountNotDetermined**	| Boolean	| Box 2b, Taxable amount not determined	|
| **totalDistribution**	| Boolean	| Box 2c, Total distribution	|
| **capitalGain**	| number (double)	| Box 3, Capital gain	|
| **federalTaxWithheld**	| number (double)	| Box 4, Federal income tax withheld	|
| **employeeContributions**	| number (double)	| Box 5, Employee contributions	|
| **netUnrealizedAppreciation**	| number (double)	| Box 6, Net unrealized appreciation	|
| **distributionCodes**	| array of string	| Box 7, Distribution codes	|
| **iraSepSimple**	| Boolean	| Box 7b, IRA/SEP/SIMPLE	|
| **otherAmount**	| number (double)	| Box 8, Other	|
| **otherPercent**	| number (double)	| Box 8, Other percent	|
| **yourPercentOfTotal**	| number (double)	| Box 9a, Your percent of total distribution	|
| **totalEmployeeContributions**	| number (double)	| Box 9b, Total employee contributions	|
| **stateTaxWithholding**	| array of StateTaxWithholding	| Boxes 12-14, State tax withholding	|
| **localTaxWithholding** | array of LocalTaxWithholding	| Boxes 15-17, Local tax withholding	|

## 14.82 Tax1099S

Proceeds from Real Estate Transactions. Extends and inherits all fields from Tax.

| **Field** | **Type** | **Description** |
|---|---|---|
| **filerNameAddress**	| NameAddressPhone	| Filer's name, address, and phone	|
| **filerTin**	| string	| FILER'S TIN	|
| **transferorTin**	| string	| TRANSFEROR'S TIN	|
| **transferorNameAddress**	| NameAddress	| Transferor's name and address	|
| **accountNumber**	| string	| Account or escrow number	|
| **dateOfClosing**	| Timestamp	| Box 1, Date of closing	|
| **grossProceeds**	| number (double)	| Box 2, Gross proceeds	|
| **addressOrLegalDescription**	| string	| Box 3, Address or legal description	|
| **receivedOtherConsideration**	| Boolean	| Box 4, Transferor received or will receive property or services as part of the consideration (if checked) |
| **foreignPerson**	| Boolean	| Box 5, If checked, transferor is a foreign person (nonresident alien, foreign partnership, foreign estate, or foreign trust)	|
| **realEstateTax**	| number (double)	| Box 6, Buyer's part of real estate tax	|

## 14.83 Tax1099Sa

Distributions from an HSA, Archer MSA, or Medicare Advantage MSA. Extends and inherits all fields from Tax.

| **Field** | **Type** | **Description** |
|---|---|---|
| **payerNameAddress**	| NameAddressPhone	| Payer's name, address, and phone	|
| **payerTin**	| string	| PAYER'S TIN	|
| **recipientTin**	| string	| RECIPIENT'S TIN	|
| **recipientNameAddress**	| NameAddress	| Recipient's name and address	|
| **accountNumber**	| string	| Account number	|
| **grossDistribution**	| number (double)	| Box 1, Gross distribution	|
| **earnings**	| number (double)	| Box 2, Earnings on excess cont.	|
| **distributionCode**	| string	| Box 3, Distribution code	|
| **fairMarketValue**	| number (double)	| Box 4, FMV on date of death	|
| **hsa**	| Boolean	| Box 5a, HSA	|
| **archerAccount**	| Boolean	| Box 5b, Archer MSA	|
| **medicalSavingsAccount**	| Boolean	| Box 5c, Medicare Advantage (MA) MSA	|

## 14.84 Tax1120SK1

Shareholder's Share of Income, Deductions, Credits, etc. Extends and inherits all fields from Tax.

| **Field** | **Type** | **Description** |
|---|---|---|
| **finalK1**	| Boolean	| Final K-1	|
| **amendedK1**	| Boolean	| Amended K-1	|
| **fiscalYearBegin**	| Timestamp	| Fiscal year begin date	|
| **fiscalYearEnd**	| Timestamp	| Fiscal year end date	|
| **corporationTin**	| string	| Box A, Corporation's employer identification number	|
| **corporationNameAddress**	| NameAddress	| Box B, Corporation's name, address, city, state, and ZIP code	|
| **irsCenter**	| string	| Box C, IRS Center where corporation filed return	|
| **shareholderTin**	| string	| Box D, Shareholder's identifying number	|
| **shareholderNameAddress**	| NameAddress	| Box E, Shareholder's name, address, city, state, and ZIP code	|
| **percentOwnership**	| number (double)	| Box F, Shareholder's percentage of stock ownership for tax year	|
| **ordinaryIncome**	| number (double)	| Box 1, Ordinary business income (loss)	|
| **netRentalRealEstateIncome**	| number (double)	| Box 2, Net rental real estate income (loss) |
| **otherRentalIncome**	| number (double)	| Box 3, Other net rental income (loss)	|
| **interestIncome**	| number (double)	| Box 4, Interest income	|
| **ordinaryDividends**	| number (double)	| Box 5a, Ordinary dividends	|
| **qualifiedDividends**	| number (double)	| Box 5b, Qualified dividends	|
| **royalties**	| number (double)	| Box 6, Royalties	|
| **netShortTermGain**	| number (double)	| Box 7, Net short-term capital gain (loss)	|
| **netLongTermGain**	| number (double)	| Box 8a, Net long-term capital gain (loss)	|
| **collectiblesGain**	| number (double)	| Box 8b, Collectibles (28%) gain (loss)	|
| **unrecaptured1250Gain**	| number (double)	| Box 8c, Unrecaptured section 1250 gain	|
| **net1231Gain**	| number (double)	| Box 9, Net section 1231 gain (loss)	|
| **otherIncome**	| array of CodeAmount	| Box 10, Other income (loss)	|
| **section179Deduction**	| number (double)	| Box 11, Section 179 deduction	|
| **otherDeductions**	| array of CodeAmount	| Box 12, Other deductions	|
| **credits**	| array of CodeAmount	| Box 13, Credits	|
| **foreignTransactions**	| array of CodeAmount	| Box 14, Foreign transactions	|
| **foreignCountry**	| string	| Box 14, Foreign country	|
| **amtItems**	| array of CodeAmount	| Box 15, Alternative minimum tax (AMT) items	|
| **basisItems**	| array of CodeAmount	| Box 16, Items affecting shareholder basis	|
| **otherInfo**	| array of CodeAmount	| Box 17, Other information	|
| **multipleAtRiskActivities** | Boolean | Box 18, More than one activity for at-risk purposes |
| **multiplePassiveActivities** | Boolean | Box 19, More than one activity for passive activity purposes |

## 14.85 Tax2439

Notice to Shareholder of Undistributed Long-Term Capital Gains. Extends and inherits all fields from Tax.

| **Field** | **Type** | **Description** |
|---|---|---|
| **fiscalYearBegin**	| Timestamp	| Fiscal year begin date	|
| **fiscalYearEnd**	| Timestamp	| Fiscal year end date	|
| **ricOrReitNameAddress**	| NameAddressPhone	| RIC or REIT's name, address and phone	|
| **ricOrReitTin**	| string	| Identification number of RIC or REIT	|
| **shareholderNameAddress**	| NameAddress	| Shareholder's address	|
| **shareholderTin**	| string	| Shareholder's identifying number	|
| **undistributedLongTermCapitalGains**	| number (double)	| Box 1a, Total undistributed long-term capital gains	|
| **unrecaptured1250Gain**	| number (double)	| Box 1b, Unrecaptured section 1250 gain	|
| **section1202Gain**	| number (double)	| Box 1c, Section 1202 gain	|
| **collectiblesGain**	| number (double)	| Box 1d, Collectibles (28%) gain	|
| **taxPaid**	| number (double)	| Box 2, Tax paid by the RIC or REIT on the box 1a gains	|

## 14.86 Tax5498

IRA Contribution Information. Extends and inherits all fields from Tax.

| **Field** | **Type** | **Description** |
|---|---|---|
| **issuerNameAddress**	| NameAddressPhone	| Issuer's name, address and phone	|
| **issuerTin**	| string	| TRUSTEE'S or ISSUER'S TIN	|
| **participantTin**	| string	| PARTICIPANT'S TIN	|
| **participantNameAddress**	| NameAddress	| Participant's name and address	|
| **accountNumber**	| string	| Account number	|
| **iraContributions**	| number (double)	| Box 1, IRA contributions	|
| **rolloverContributions**	| number (double)	| Box 2, Rollover contributions	|
| **rothIraConversion**	| number (double)	| Box 3, Roth IRA conversion amount	|
| **recharacterizedContributions**	| number (double)	| Box 4, Recharacterized contributions	|
| **fairMarketValue**	| number (double)	| Box 5, Fair market value of account	|
| **lifeInsuranceCost**	| number (double)	| Box 6, Life insurance cost included in box 1	|
| **ira**	| Boolean	| Box 7a, IRA	|
| **sep**	| Boolean	| Box 7b, SEP	|
| **simple**	| Boolean	| Box 7c, SIMPLE	|
| **rothIra**	| Boolean	| Box 7d, ROTHIRA	|
| **sepContributions**	| number (double)	| Box 8, SEP contributions	|
| **simpleContributions**	| number (double)	| Box 9, SIMPLE contributions	|
| **rothIraContributions**	| number (double)	| Box 10, Roth IRA contributions	|
| **rmdNextYear**	| Boolean	| Box 11, If checked, required minimum distribution for 2020	|
| **rmdDate**	| Timestamp	| Box 12a, RMD date	|
| **rmdAmount**	| number (double)	| Box 12b, RMD amount	|
| **postponedContribution**	| number (double)	| Box 13a, Postponed contribution	|
| **postponedYear**	| integer (int32)	| Box 13b, Year	|
| **postponedCode**	| string	| Box 13c, Code	|
| **repayments**	| number (double)	| Box 14a, Repayments	|
| **repayCode**	| string	| Box 14b, Code	|
| **fmvSpecifiedAssets**	| number (double)	| Box 15a, FMV of certain specified assets	|
| **specifiedCodes**	| string	| Box 15b, Code(s)	|

## 14.87 Tax5498Esa

Coverdell ESA Contribution Information. Extends and inherits all fields from Tax.

| **Field** | **Type** | **Description** |
|---|---|---|
| **issuerNameAddress**	| NameAddressPhone	| Issuer's name, address, and phone	|
| **issuerTin**	| string	| TRUSTEE'S/ISSUER'S TIN	|
| **participantTin**	| string	| BENEFICIARY'S TIN	|
| **beneficiaryNameAddress**	| NameAddress	| Beneficiary's name and address	|
| **accountNumber**	| string	| Account number	|
| **coverdellEsaContributions**	| number (double)	| Box 1, Coverdell ESA contributions	|
| **rolloverContributions**	| number (double)	| Box 2, Rollover contributions	|

## 14.88 Tax5498Sa

HSA, Archer MSA, or Medicare Advantage (MA) MSA Information. Extends and inherits all fields from Tax.

| **Field** | **Type** | **Description** |
|---|---|---|
| **trusteeNameAddress**	| NameAddressPhone	| Trustee's name, address, and phone	|
| **trusteeTin**	| string	| TRUSTEE'S TIN	|
| **participantTin**	| string	| PARTICIPANT'S TIN	|
| **participantNameAddress**	| NameAddress	| Participant's name and address	|
| **accountNumber**	| string	| Account number	|
| **msaContributions**	| number (double)	| Box 1, Employee or self-employed person's Archer MSA contributions made in 2019 and 2020 for 2019	|
| **totalContributions**	| number (double)	| Box 2, Total contributions made in 2019	|
| **totalPostYearEnd**	| number (double)	| Box 3, Total HSA or Archer MSA contributions made in 2020 for 2019	|
| **rolloverContributions**	| number (double)	| Box 4, Rollover contributions	|
| **fairMarketValue**	| number (double)	| Box 5, Fair market value of HSA, Archer MSA, or Medicare Advantage (MA) MSA	|
| **hsa**	| Boolean	| Box 6a, HSA	|
| **archer**	| Boolean	| Box 6b, Archer MSA	|
| **maMsa**	| Boolean	| Box 6c, Medicare Advantage (MA) MSA	|

## 14.89 TaxW2

Wage and Tax Statement. Extends and inherits all fields from Tax.

| **Field** | **Type** | **Description** |
|---|---|---|
| **employeeTin**	| string	| Employee's social security number	|
| **employerTin**	| string	| Employer identification number (EIN)	|
| **employerNameAddress**	| NameAddress	| Employer's name and address	|
| **controlNumber**	| string	| Control number	|
| **employeeName**	| IndividualName	| Employee name	|
| **employeeAddress**	| Address	| Employee's address	|
| **wages**	| number (double)	| Box 1, Wages, tips, other compensation	|
| **federalTaxWithheld**	| number (double)	| Box 2, Federal income tax withheld	|
| **socialSecurityWages**	| number (double)	| Box 3, Social security wages	|
| **socialSecurityTaxWithheld**	| number (double)	| Box 4, Social security tax withheld	|
| **medicareWages**	| number (double)	| Box 5, Medicare wages and tips	|
| **medicareTaxWithheld**	| number (double)	| Box 6, Medicare tax withheld	|
| **socialSecurityTips**	| number (double)	| Box 7, Social security tips	|
| **allocatedTips**	| number (double)	| Box 8, Allocated tips	|
| **dependentCareBenefit**	| number (double)	| Box 10, Dependent care benefits	|
| **nonQualifiedPlan**	| number (double)	| Box 11, Nonqualified plans	|
| **codes**	| array of CodeAmount	| Box 12, Codes and amounts	|
| **statutory**	| Boolean	| Box 13, Statutory employee	|
| **retirementPlan**	| Boolean	| Box 13, Retirement plan	|
| **thirdPartySickPay**	| Boolean	| Box 13, Third-party sick pay	|
| **other** | array of DescriptionAmount	| Box 14, Other descriptions and amounts	|
| **stateTaxWithholding**	| array of StateTaxWithholding	| Boxes 15-17, State tax withholding	|
| **localTaxWithholding**	| array of LocalTaxWithholding	| Boxes 18-20, Local tax withholding	|

## 14.89 TaxW2G

Certain Gambling Winnings. Extends and inherits all fields from Tax.

| **Field** | **Type** | **Description** |
|---|---|---|
| **payerNameAddress**	| NameAddressPhone	| Payer's name, address and phone	|
| **payerTin**	| string	| PAYER'S federal identification number	|
| **winnerNameAddress**	| NameAddress	| Winner's name and address	|
| **winnings**	| number (double)	| Box 1, Reportable winnings	|
| **dateWon**	| Timestamp	| Box 2, Date won	|
| **typeOfWager**	| string	| Box 3, Type of wager	|
| **federalTaxWithheld**	| number (double)	| Box 4, Federal income tax withheld	|
| **transaction**	| string	| Box 5, Transaction	|
| **race**	| string	| Box 6, Race	|
| **identicalWinnings**	| number (double)	| Box 7, Winnings from identical wagers	|
| **cashier**	| string	| Box 8, Cashier	|
| **winnerTin**	| string	| Box 9, Winner's taxpayer identification no.	|
| **window**	| string	| Box 10, Window	|
| **firstId**	| string	| Box 11, First I.D.	|
| **secondId**	| string	| Box 12, Second I.D.	|
| **payerState**	| string	| Box 13, State	|
| **stateWinnings**	| number (double)	| Box 14, State winnings	|
| **stateTaxWithheld**	| number (double)	| Box 15, State income tax withheld	|
| **localWinnings**	| number (double)	| Box 16, Local winnings	|
| **localTaxWithheld**	| number (double)	| Box 17, Local income tax withheld	|
| **localityName**	| string	| Box 18, Name of locality	|
| **payerStateId**	| string	| Box 13, Payer's state identification no.	|

## 14.90 Tax

| **Field** | **Type** | **Description** |
|---|---|---|
| **taxYear** | integer | Year for which taxes are being paid |
| **corrected** | Boolean | True to indicate this is a corrected tax form |
| **accountId** | Identifier | Long-term persistent identity of the source account. Not the account number. |
| **taxFormId** | Identifier | Long-term persistent id for the tax form instance |
| **taxFormDate** | Timestamp | Date of production or delivery of the tax form |
| **description** | string | Description of the tax document |
| **additionalInformation** | string | Additional explanation text or content for taxpayer or preparer or IRS about the tax document |
| **taxFormType** | TaxFormType | Enumerated name of the tax form entity e.g. "TaxW2" |
| **self** | HateoasLink | |

## 14.91 TaxDataForQR

Tax data container for QR Code purposes.  Inherits and extends all of TaxData.

| **Field** | **Type** | **Description** |
|---|---|---|
| **version** | string | Financial Data Exchange (FDX) schema version number (e.g. 4.0). See https://financialdataexchange.org/. |
| **softwareId** | string | Id of company or software generating this tax data |

## 14.92 TaxData

Tax data container for API.  Contains one and only one of the listed forms.

| **Field** | **Type** | **Description** |
|---|---|---|
| **tax1095A** | Tax1095A | Health Insurance Marketplace Statement |
| **tax1095B** | Tax1095B | Health Coverage |
| **tax1095C** | Tax1095C | Employer-Provided Health Insurance Offer and Coverage |
| **tax1041K1** | Tax1041K1 | Beneficiary's Share of Income, Deductions, Credits, etc. |
| **tax1065K1** | Tax1065K1 | Partner's Share of Income, Deductions, Credits, etc. |
| **tax1097Btc** | Tax1097Btc | Bond Tax Credit |
| **tax1098** | Tax1098 | Mortgage Interest Statement |
| **tax1098C** | Tax1098C | Contributions of Motor Vehicles, Boats, and Airplanes |
| **tax1098E** | Tax1098E | Student Loan Interest Statement |
| **tax1098T** | Tax1098T | Tuition Statement |
| **tax1099A** | Tax1099A | Acquisition or Abandonment of Secured Property |
| **tax1099B** | Tax1099B | Proceeds From Broker and Barter Exchange Transactions |
| **tax1099C** | Tax1099C | Cancellation of Debt |
| **tax1099Cap** | Tax1099Cap | Changes in Corporate Control and Capital Structure |
| **tax1099Div** | Tax1099Div | Dividends and Distributions |
| **tax1099G** | Tax1099G | Certain Government Payments |
| **tax1099H** | Tax1099H | Health Coverage Tax Credit (HCTC) Advance Payments |
| **tax1099Int** | Tax1099Int | Interest Income |
| **tax1099K** | Tax1099K | Merchant Card and Third-Party Network Payments |
| **tax1099Ltc** | Tax1099Ltc | Long-Term Care and Accelerated Death Benefits |
| **tax1099Misc** | Tax1099Misc | Miscellaneous Income |
| **tax1099Oid** | Tax1099Oid | Original Issue Discount |
| **tax1099Patr** | Tax1099Patr | Taxable Distributions Received From Cooperatives |
| **tax1099Q** | Tax1099Q | Payments From Qualified Education Programs |
| **tax1099R** | Tax1099R | Distributions from Pensions, Annuities, Retirement or Profit-Sharing Plans, IRAs, Insurance Contracts, etc. |
| **tax1099S** | Tax1099S | Proceeds From Real Estate Transactions |
| **tax1099Sa** | Tax1099Sa | Distributions From an HSA, Archer MSA, or Medicare Advantage MSA |
| **tax1120SK1** | Tax1120SK1 | Shareholder's Share of Income, Deductions, Credits, etc. |
| **tax2439** | Tax2439 | Notice to Shareholder of Undistributed Long-Term Capital Gains |
| **tax5498** | Tax5498 | IRA Contribution Information |
| **tax5498Esa** | Tax5498Esa | Coverdell ESA Contribution Information |
| **tax5498Sa** | Tax5498Sa | HSA, Archer MSA, or Medicare Advantage MSA Information |
| **taxW2** | TaxW2 | Wage and Tax Statement |
| **taxW2G** | TaxW2G | Certain Gambling Winnings |
| **taxRefundDirectDeposit** | TaxRefundDirectDeposit | Tax refund direct deposit information |

## 14.93 TaxDataList

Paginated array of tax data.  Inherits and extends all of PaginatedArray.

| **Field** | **Type** | **Description** |
|---|---|---|
| **forms** | Array of TaxData | |

## 14.94 TaxRefundDirectDeposit

IRS Form 8888 Direct Deposit Information

| **Field** | **Type** | **Description** |
|---|---|---|
| **institutionName** | string | Name of institution |
| **rtn** | string | Routing transit number |
| **accountNumber** | string | Account number |
| **accountNickName** | string | Account nickname |

## 14.95 AccountWithDetails

An instance of an account with full details.

| **Field** | **Type** | **Description** |
|---|---|---|
|one of **depositAccount**, **investmentAccount**, **loanAccount**, **locAccount**, **annuityAccount**, **insuranceAccount** |One of DepositAccount, LoanAccount, LocAccount, InvestmentAccount, InsuranceAccount or AnnuityAccount||

## 14.96 IndividualName
| **Field** | **Type** | **Description** |
|---|---|---|
| **first** | string | First name |
| **middle** | string | Middle initial |
| **last** | string | Last name |
| **suffix** | string | Generational or academic suffix |

## 14.97 LocalTaxWithholding

Local Tax Withholding Info

| **Field** | **Type** | **Description** |
|---|---|---|
| **localTaxWithheld** | number | Amount of local income tax withheld |
| **localityName** | string | Locality Name |
| **state** | string | State code of state in which locality exists |
| **localIncome** | number | Income amount for local tax purposes |

## 14.99 Address

| **Field** | **Type** | **Description** |
|---|---|---|
| **line1** | String64 | Address Line 1 |
| **line2** | String64 | Address Line 2 |
| **line3** | String64 | Address Line 3 |
| **city** | String64 | City |
| **state** | String64 | State or province |
| **postalCode** | String10 | Postal code |
| **country** | Iso3166CountryCode | Country code |

## 14.100 NameAddress
 
Inherits and extends Address

| **Field** | **Type** | **Description** |
|---|---|---|
| **name1** | String64 | Name line 1 |
| **name2** | String64 | Name line 2 |

## 14.101 NameAddressPhone

Inherits and extends NameAddress

| **Field** | **Type** | **Description** |
|---|---|---|
| **phone** | TelephoneNumberPlusExtension | |

## 14.102 TelephoneNumberPlusExtension

A telephone number that can contain optional text for an arbitrary length telephone extension number.  Inherits and extends TelephoneNumber.

| **Field** | **Type** | **Description** |
|---|---|---|
| **extension** | string | An arbitrary length telephone number extension |

## 14.102 CodeAmount

Code and amount pair used on IRS W-2, K-1, etc.

| **Field** | **Type** | **Description** |
|---|---|---|
| **code** | string | |
| **amount** | number | |

## 14.103 DescriptionAmount

Description and amount pair used on IRS W-2, etc.

| **Field** | **Type** | **Description** |
|---|---|---|
| **description** | string | |
| **amount** | number | |

## 14.104 HealthInsuranceCoverage

Used on Form 1095-A Part III

| **Field** | **Type** | **Description** |
|---|---|---|
| **enrollmentPremium** | number | Monthly enrollment premiums |
| **slcspPremium** | number | Monthly second lowest cost silver plan (SLCSP) premium |
| **advancePremiumTaxCreditPayment** | number | Monthly advance payment of premium tax credit |
| **month** | CoverageMonth | Month of coverage |

## 14.105 HealthInsuranceCoveredIndividual

Used on Form 1095-B Part IV and Form 1095-C Part III

| **Field** | **Type** | **Description** |
|---|---|---|
| **name** | IndividualName | Name of responsible individual |
| **tin** | string | Social security number or other TIN |
| **dateOfBirth** | Timestamp | |
| **coveredAt12Months** | Boolean | Covered 12 months |
| **coveredMonths** | Array of MonthAbbreviation | Months covered |

## 14.106 HealthInsuranceMarketplaceCoveredIndividual

Used on Form 1095-A Part II

| **Field** | **Type** | **Description** |
|---|---|---|
| **name** | string | Covered individual name |
| **tin** | string | Covered individual SSN |
| **dateOfBirth** | Timestamp | |
| **policyStartDate** | Timestamp | Coverage start date |
| **policyTerminationDate** | Timestamp | Coverage termination date |

## 14.107 MonthAmount

Month and amount pair used on IRS Form 1099-K, etc.

| **Field** | **Type** | **Description** |
|---|---|---|
| **month** | MonthAbbreviation | |
| **amount** | number | |

## 14.108 OfferOfHealthInsuranceCoverage

Offer Of Health Insurance Coverage

| **Field** | **Type** | **Description** |
|---|---|---|
| **coverageCode** | string | Offer of Coverage (enter required code) |
| **requiredContribution** number | Employee required contribution |
| **section4980HCode** | string | Section 4980H Safe Harbor and Other Relief (enter code) |
| **month** | CoverageMonth | | 

## 14.109 SecurityDetail

Security Detail, IRS Form 1099-B

| **Field** | **Type** | **Description** |
|---|---|---|
| **checkboxOnForm8949** | string | Applicable checkbox on Form 8949 |
| **securityName** | string | Security name | 
| **numberOfShares** | number | Number of shares |
| **saleDescription** | string | Box 1a, Description of property |
| **dateAcquired** | Timestamp | Box 1b, Date acquired |
| **variousDatesAcquired** | Boolean | Box 1b, Date acquired Various |
| **dateOfSale** | Timestamp | Box 1c, Date sold or disposed |
| **salesPrice** | number | Box 1d, Proceeds (not price per share) |
| **accruedMarketDiscount** | number | Box 1f, Accrued market discount |
| **adjustmentCodes** | array of CodeAmount | Other adjustments (code and amount) |
| **costBasis** | number | Box 1e, Cost or other basis |
| **washSaleLossDisallowed** | number | Box 1g, Wash sale loss disallowed |
| **longOrShort** | SaleTermType | Box 2, LONG or SHORT |
| **ordinary** | Boolean | Box 2, Ordinary |
| **collectible** | Boolean | Box 3, Collectibles |
| **qof** | Boolean | Box 3, Qualified Opportunity Fund (QOF) |
| **federalTaxWithheld** | number | Box 4, Federal income tax withheld |
| **noncoveredSecurity** | Boolean | Box 5, Noncovered security |
| **grossOrNet** | SaleProceedsType | Box 6, Reported to IRS: GROSS or NET |
| **lossNotAllowed** | Boolean | Box 7, Loss not allowed based on proceeds |
| **basisReported** | Boolean | Box 12, Basis reported to IRS |
| **stateTaxWithholding** | array of StateTaxWithholding | Boxes 14-16, State tax withholding |
| **cusip** | string | CUSIP number |
| **foreignAccountTaxCompliance** | Boolean | Foreign account tax compliance |

## 14.110 StateTaxWithholding

State Tax Withholding Info

| **Field** | **Type** | **Description** |
|---|---|---|
| **stateTaxWithheld** | number | Amount of state income tax withheld |
| **state** | string | State two-digit code |
| **stateTaxId** | string | Tax id of company withholding state income tax |
| **stateIncome** | number | Income amount for state tax purposes |

# 15 Simple Types

Simple types are specified in the accompanying FDX API Specification document fdxapi4.yaml. All defined simple types inherit from the following basic types.

| **Name** | **Description**                       |
|----------|---------------------------------------|
| string   | A string of Unicode characters        |
| int      | A 32-bit signed integer               |
| decimal  | An arbitrary-precision decimal number |
| dateTime | A string specifying a date and time   |

Many enumerations are implemented without enforcement of values to accommodate categorizations that are not covered by this document. These are described as having **Suggested Values**. Enumerations whose values are enforced are described as having **Valid Values**.

## 15.1 AccountStatus

### Base Type
string

### Suggested Values
- CLOSED
- DELINQUENT
- NEGATIVECURRENTBALANCE
- OPEN
- PAID
- PENDINGCLOSE
- PENDINGOPEN

## 15.2 AccountType

### Base Type
string

### Suggested Values
- 401A
- 401K
- 403B
- 529
- AUTOLOAN
- CD
- CHARGE
- CHECKING
- COMMERCIALLINEOFCREDIT
- COMMERCIALLOAN
- COVERDELL
- CREDITCARD
- ESCROW
- ESOP
- GUARDIAN
- HOMEEQUITYLOAN
- HOMELINEOFCREDIT
- INSTITUTIONALTRUST
- INSTALLMENT
- IRA
- KEOGH
- LINEOFCREDIT
- LOAN
- MILITARYLOAN
- MONEYMARKET
- MORTGAGE
- PERSONALLOAN
- ROLLOVER
- ROTH
- SARSEP
- SAVINGS
- SMBLOAN
- STUDENTLOAN
- TAXABLE
- TDA
- TRUST
- UGMA
- UTMA
- ANNUITY

## 15.3 AnnuityProductType

### Base Type
string

### Suggested Values
- CURRENCY
- SHARES

## 15.4 AnnuityValueBasis

### Base Type
string

### Suggested Values
- FIXED
- VARIABLE

## 15.5 AnnualIncreaseType

### Base Type
string

### Suggested Values
- FIXED
- PERCENT
- DOLLAR

## 15.6 AssetClass

### Base Type
string

### Suggested Values
- DOMESTICBOND
- INTLBOND
- INTLSTOCK
- LARGESTOCK
- MONEYMARKET
- OTHER
- SMALLSTOCK

## 15.7 BalanceType

### Definitions
- ASSET (positive transaction amount increases balance)
- LIABILITY (positive transaction amount decreases balance)

### Base Type
string

### Valid Values
- ASSET
- LIABILITY

## 15.8 Boolean

'true' or 'false'

### Base Type
boolean

## 15.9 CallType

### Base Type
string

### Suggested Values
- CALL
- MATURITY
- PREFUND
- PUT

## 15.10 CompoundingPeriod

### Base Type
string

### Suggested Values
- ANNUALLY
- BIWEEKLY
- DAILY
- MONTHLY
- SEMIANNUALLY
- SEMIMONTHLY
- WEEKLY

## 15.11 CouponMatureFrequency

### Base Type
string

### Suggested Values
- ANNUAL
- MONTHLY
- OTHER
- QUARTERLY
- SEMIANNUAL

## 15.12 DebitCreditMemo

### Base Type
string

### Suggested Values
- CREDIT
- DEBIT
- MEMO

## 15.13 DebtClass

### Base Type
String

### Suggested Values
- CORPORATE
- MUNICIPAL
- OTHER
- TREASURY

## 15.14 DebtType

### Base Type
String

### Suggested Values
- COUPON
- ZERO

## 15.15 DeliveryAddressType

### Base Type
string

### Suggested Values
- BUSINESS
- HOME
- MAILING

## 15.16 DepositTransactionType

### Base Type
string

### Suggested Values
- ADJUSTMENT
- ATMDEPOSIT
- ATMWITHDRAWAL
- BILLPAYMENT
- CHECK
- DEPOSIT
- DIRECTDEPOSIT
- DIVIDEND
- FEE INTEREST
- POSCREDIT
- POSDEBIT
- TRANSFER
- WITHDRAWAL

## 15.17 HeldInAccount

### Base Type
string

### Suggested Values
- CASH
- MARGIN
- OTHER
- SHORT

## 15.18 HoldingSubType

### Base Type
string

### Suggested Values
- CASH
- MONEYMARKET

## 15.19 HoldingType

### Base Type
string

### Suggested Values
- ANNUITY
- BOND
- CD
- MUTUALFUND
- OPTION
- OTHER
- STOCK

## 15.20 Identifier

### Base Type
string

### Maximum Length
256

## 15.21 IncomeType

### Base Type
string

### Suggested Values
- CGLONG
- CGSHORT
- MISC

## 15.22 InterestRateType

### Base Type
string

### Suggested Values
- FIXED
- VARIABLE

## 15.23 Inv401kSourceType

### Base Type
string

### Suggested Values
- AFTERTAX
- MATCH
- OTHERNONVEST
- OTHERVEST
- PRETAX
- PROFITSHARING
- ROLLOVER

## 15.24 InvestmentBalanceType

### Base Type
string

### Suggested Values
- AMOUNT
- PERCENTAGE

## 15.25 InvestmentTransactionType

### Base Type
string

### Suggested Values
- ADJUSTMENT
- ATM
- CASH
- CHECK
- CLOSURE
- CLOSUREOPT
- CONTRIBUTION
- DEP
- DEPOSIT
- DIRECTDEBIT
- DIRECTDEP
- DIV
- DIVIDEND
- DIVIDENDREINVEST
- EXPENSE
- FEE
- INCOME
- INTEREST
- INVEXPENSE
- JRNLFUND
- JRNLSEC
- MARGININTEREST
- OPTIONEXERCISE
- OPTIONEXPIRATION
- OTHER
- PAYMENT
- POS
- PURCHASED
- PURCHASEDTOCOVER
- PURCHASETOCLOSE
- PURCHASETOOPEN
- REINVESTOFINCOME
- REPEATPMT
- RETURNOFCAPITAL
- SOLD
- SOLDTOCLOSE
- SOLDTOOPEN
- SPLIT
- SRVCHG
- TRANSFER
- XFER

## 15.26 Iso3166CountryCode

ISO 3166 Codes for the representation of names of countries and their
subdivisions.

### Base Type
string

### Valid Values
- AD AE AF AG AI AL AM AN AO AQ AR AS AT AU AW AX AZ
- BA BB BD BE BF BG BH BI BJ BM BN BO BR BS BT BV BW BY BZ
- CA CC CD CF CG CH CI CK CL CM CN CO CR CS CU CV CX CY CZ
- DE DJ DK DM DO DZ
- EC EE EG EH ER ES ET
- FI FJ FK FM FO FR
- GA GB GD GE GF GG GH GI GL GM GN GP GQ GR GS GT GU GW GY
- HK HM HN HR HT HU
- ID IE IL IM IN IO IQ IR IS IT
- JE JM JO JP
- KE KG KH KI KM KN KP KR KW KY KZ
- LA LB LC LI LK LR LS LT LU LV LY
- MA MC MD MG MH MK ML MM MN MO MP MQ MR MS MT MU MV MW MX MY MZ
- NA NC NE NF NG NI NL NO NP NR NU NZ
- OM
- PA PE PF PG PH PK PL PM PN PR PS PT PW PY
- QA
- RE RO RU RW
- SA SB SC SD SE SG SH SI SJ SK SL SM SN SO SR ST SV SY SZ
- TC TD TF TG TH TJ TK TL TM TN TO TR TT TV TW TZ
- UA UG UM US UY UZ
- VA VC VE VG VI VN VU
- WF WS
- YE YT
- ZA ZM ZW

## 15.27 Iso4217Code

### Base Type
string

### Valid Values
- AED AFN ALL AMD ANG AOA ARS AUD AWG AZN
- BAM BBD BDT BGN BHD BIF BMD BND BOB BOV BRL BSD BTN BWP BYR BZD
- CAD CDF CHE CHF CHW CLF CLP CNY COP COU CRC CUC CUP CVE CZK
- DJF DKK DOP DZD
- EGP ERN ETB EUR
- FJD FKP
- GBP GEL GHS GIP GMD GNF GTQ GYD
- HKD HNL HRK HTG HUF
- IDR ILS INR IQD IRR ISK
- JMD JOD JPY
- KES KGS KHR KMF KPW KRW KWD KYD KZT
- LAK LBP LKR LRD LSL LYD
- MAD MDL MGA MKD MMK MNT MOP MRO MUR MVR MWK MXN MXV MYR MZN
- NAD NGN NIO NOK NPR NZD
- OMR
- PAB PEN PGK PHP PKR PLN PYG
- QAR
- RON RSD RUB RWF
- SAR SBD SCR SDG SEK SGD SHP SLL SOS SRD SSP STD SVC SYP SZL
- THB TJS TMT TND TOP TRY TTD TWD TZS
- UAH UGX USD USN UYI UYU UZS
- VEF VND VUV
- WST
- XAF XAG XAU XBA XBB XBC XBD XCD XDR XOF XPD XPF XPT XSU XTS XUA XXX
- YER
- ZAR ZMW ZWL

## 15.28 LoanPaymentFrequency

### Base Type
string

### Suggested Values
- ANNUALLY
- BIMONTHLY
- BIWEEKLY
- FOURWEEKS
- MONTHLY
- OTHER
- QUARTERLY
- SEMIANNUALLY
- TWICEMONTHLY
- WEEKLY

## 15.29 LoanTransactionType

### Base Type
string

### Suggested Values
- ADJUSTMENT
- FEE
- INTEREST
- PAYMENT

## 15.30 LocTransactionType

### Base Type
string

### Suggested Values
- ADJUSTMENT
- CHECK
- FEE
- INTEREST
- PAYMENT
- WITHDRAWAL

## 15.31 MessageFormat

### Base Type
string

### Valid Values
- JSON

## 15.32 MutualFundType

### Base Type
string

### Suggested Values
- CLOSEEND
- OPENEND
- OTHER

## 15.33 Number

### Base Type
number

## 15.34 OptionType

### Base Type
string

### Suggested Values
- CALL
- PUT

## 15.35 OrderDuration

### Base Type
string

### Suggested Values
- DAY
- GOODTILLCANCEL
- IMMEDIATE

## 15.36 OrderType

### Base Type
string

### Suggested Values
- BUY
- BUYTOCOVER
- BUYTOOPEN
- SELL
- SELLCLOSE
- SELLSHORT
- SELLTOCOVER
- SELLTOOPEN

## 15.37 PaymentFrequency

### Base Type
string

### Suggested Values
- ANNUALLY
- BIWEEKLY
- DAILY
- MONTHLY
- SEMIANNUALLY
- SEMIMONTHLY
- WEEKLY

## 15.38 PeriodCertainGuarantee

### Base Type
string

### Suggested Values
- NO PERIOD CERTAIN
- 5-YEAR
- 10-YEAR
- 20-YEAR
- 30-YEAR

## 15.39 PositionType

### Base Type
string

### Suggested Values
- LONG
- SHORT

## 15.40 Secured

### Base Type
string

### Suggested Values
- COVERED
- NAKED

## 15.41 SecurityIdType

### Base Type
string

### Suggested Values
- CUSIP
- ISIN
- SEDOL
- SICC
- VALOR
- WKN

## 15.42 SecurityType

### Base Type
string

### Suggested Values
- BOND
- DEBT
- MUTUALFUND
- OPTION
- OTHER
- STOCK
- SWEEP

## 15.43 StockType

### Base Type
string

### Suggested Values
- COMMON
- CONVERTIBLE
- OTHER
- PREFERRED

## 15.44 String10
### Base Type
string
### Maximum Length
10

## 15.45 String2
### Base Type
string
### Maximum Length
2

## 15.46 String255

### Base Type
string

### Maximum Length
255

## 15.47 String3

### Base Type
string

### Maximum Length
3

## 15.48 String64

### Base Type
string

### Maximum Length
64

## 15.49 String9

### Base Type
string

### Maximum Length
9

## 15.50 SubAccountType

### Base Type
string

### Suggested Values
- CASH
- MARGIN
- OTHERS
- SHORT

## 15.51 TelephoneNumberType

### Base Type
string

### Suggested Values
- BUSINESS
- CELL
- FAX
- HOME

## 15.52 Timestamp

ISO 8601 date time with milliseconds in UTC time zone.

### Base Type
dateTime

## 15.53 TransactionReason

### Base Type
string

### Suggested Values
- CALL
- MATURITY
- SELL

## 15.54 TransactionStatus

### Base Type
string

### Suggested Values
- AUTHORIZATION
- MEMO
- PENDING
- POSTED

## 15.55 TransferStatusStatus

### Base Type
string

### Suggested Values
- FAILURE
- NOFUNDS
- PENDING
- SUCCESS

## 15.56 UnitType

### Base Type
string

### Suggested Values
- CURRENCY
- SHARES

## 15.57 CoverageMonth

### Description
Used by Form 1095-A

### BaseType
string

### Suggested Values
- ANNUAL
- JANUARY
- FEBRUARY
- MARCH
- APRIL
- MAY
- JUNE
- JULY
- AUGUST
- SEPTEMBER
- OCTOBER
- NOVEMBER
- DECEMBER

## 15.58 MonthAbbreviation

### Description
Used by MonthAmount

### BaseType
 string

### Suggested Values
- JAN
- FEB
- MAR
- APR
- MAY
- JUN
- JUL
- AUG
- SEP
- OCT
- NOV
- DEC

## 15.59 SaleProceedsType

### Description
Gross or net proceeds. Used by Form 1099-B.

### BaseType
string

### Suggested Values
- GROSS
- NET

## 15.60 SaleTermType

### Description
Long or short term. Used by Form 1099-B

### BaseType
string

### Suggested Values
- LONG
- SHORT

## 15.61 TaxFormType

### Description
Tax form entity name e.g. "TaxW2"

### BaseType
string

### Suggested Values
- Tax1041K1
- Tax1065K1
- Tax1095A
- Tax1095B
- Tax1095C
- Tax1097Btc
- Tax1098
- Tax1098C
- Tax1098E
- Tax1098T
- Tax1099A
- Tax1099B
- Tax1099C
- Tax1099Cap
- Tax1099Div
- Tax1099G
- Tax1099H
- Tax1099Int
- Tax1099K
- Tax1099Ltc
- Tax1099Misc
- Tax1099Oid
- Tax1099Patr
- Tax1099Q
- Tax1099R
- Tax1099S
- Tax1099Sa
- Tax1120SK1
- Tax2439
- Tax5498
- Tax5498Esa
- Tax5498Sa
- TaxW2
- TaxW2G

## 15.62 ContentTypes
### Description
Types of document formats
### BaseType
string
### Suggested Values
- application/pdf
- image/gif
- image/jpeg
- image/tiff
- image/png
- application/json
### Example
application/json

## 15.63 String15
### BaseType
string
### MaximumLength
15
