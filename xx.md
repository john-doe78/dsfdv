<a href="https://www.youtube.com/playlist?list=PL_XxuZqN0xVAWGDKIzcn6NWikVkljJQZc"> <img src="https://i.ibb.co/KqswBh7/image.png" alt="rest-api-design-workshop"> </a>

### API Definition

An API is a contract between two parties:

* **Consumer**: The entity that consumes or uses the API.
* **Producer**: The entity that creates or provides the API.

### API Architectural Style

* SOAP (Simple Object Access Protocol)
* REST (Representational State Transfer)
* gRPC (Remote Procedure Calls)
* Web Hooks
* GraphQL

### Importance of API Design

* Interoperability
* Abstraction
* Reusability
* Adaptability
* Developer Experience
* Security & Reliability

### Public Docs

> API first approach priorities APIs at the beginning of the software development process.

* https://woocommerce.github.io/woocommerce-rest-api-docs/
* https://docs.github.com/en/rest/
* https://developer.spotify.com/documentation/web-api

### Types of API Consumers

* **Public**: Open to anyone.
* **Private**: Restricted to internal use.
* **Partner**: Shared privately between two parties.

### API Life Cycle

![API Life Cycle](https://voyager.postman.com/infographics/producer-consumer-api-lifecycle-postman.png)

## What is API Design?

API design is the process of making intentional decisions about how an API will expose data and functionality to its consumers. A successful API design describes the API endpoints, methods, and resources in a standardized specification format.

#### Design Process

* Define what the API is intended to do.
* Create a specification outlining the API’s endpoints, methods, and data formats.
* Use mocks and tests to ensure the API behaves as expected.
* Provide clear documentation for developers to understand and use the API.

### What is your API?

* REST API
* REST like API

### What is REST API?

REST stands for *Representational State Transfer* and API stands for *Application Programming Interface*. It's an architectural style for networked applications, defining principles for resource identification, addressing, and data exchange between clients and servers via HTTP.

* **Resource**: Any data object or entity that can be accessed or manipulated through rest API.
* **Representational State**: Represents the current state of resources, which can be in formats like XML or JSON.

### Constraints of REST API

* Client-Server
* Cacheability (cache control header)
* Uniform Interface
	* Identification of resources
	* Manipulation of resources through representations
	* Self descriptive messages
	* HATEOAS (Hypermedia As The Engine Of Application State)
* Layered System
* Code-on-demand
* Statelessness

### How Do We Measure API?

![Measuring API](https://dev-to-uploads.s3.amazonaws.com/i/aregpuzds2v57k4shgpa.PNG)

#### Richardson Maturity Model

* https://restfulapi.net/richardson-maturity-model/
* https://blog.restcase.com/4-maturity-levels-of-rest-api-design/
* https://martinfowler.com/articles/richardsonMaturityModel.html

### Partial Response

Partial response allows API users to specify which fields they want in the response.

```py
GET - http://127.0.0.1:8000/api/v1/products?fields=name,photo,price
```

#### Benefits

* Reduces bandwidth usage
* Improves performance
* Optimizes client-side performance
* Simplifies API consumption
* Reduces over-fetching

### Query Parameters

Query parameters are a fundamental aspect of HTTP requests used to specify additional information for a resource retrieval or manipulation operation. They are appended to the URL of the request and consist of a key-value pair separated by an equal sign (=) and delimited by an ampersand (&) if multiple parameters are present.

* **Filtering**: `?min_price=600&max_price=1200`
* **Sorting**: `?sort=price&order=desc`
* **Searching**: `?search=iphone`
* **Pagination**: `?limit=10&offset=1`
* **Partial Fields**: `?fields=name,product,price`

### Well-Structured Response Body

#### GET

```py
{
	"status": 200,
	"message": "",
	"information": "",
	"data": {
		"id": "uuid",
		"links": {
			"self": ...,
			"update": ...,
			"delete": ...,
			"add_to_cart": ...
		}
	},
	"trace_id": "456789"
}
```

#### POST

```py
{
	"status": 201,
	"message": "Created Successfully",
	"data": {
			"id": "uuid",
			"links": {
				"update": ...,
				"delete": ...,
				"get": ...,
				"self": ...
			}
		},
	"trace_id": "789"
}
```

#### GET All Products

```py
{
	"status": 200,
	"message": "",
	"information": "",
	"data": [
		{
			"id": "uuid",
			"links": {
				"self": "..."
			}
		}
	],
	"pagination": {
		"offset": 5,
		"limit": 2,
		"total_pages": 20,
		"total_items": 100,
		"links": {
			"self": "...",
			"first": "...",
			"last": null,
			"prev": 1,
			"next": 3
		}
	}
}
```

## Error Response

* Use Standard HTTP Status Codes
	* 400 - Bad request
	* 401 - Unauthorized
	* 403 - Forbidden
	* 404 - Not found
	* 422 - Unprocessable Content
	* 500 - Internal server error
* Provide Descriptive Error Messages
* Follow a Consistent Error Message Format
* Include Error Details
* Handle Uncaught Exceptions Gracefully
* Offer Guidance for Recovery
* Response Body Should Includes
	* code
	* message
	* hints
	* trace_id

```py
{
	"data": {},
	"message": ...,
	"errors": [],
	"hints": "",
	"trace_id": "124588"
}
```

## HTTP Cache-Control

![Cache-Control](https://i.ibb.co/wsV27C4/image.png)

### Directives

* **Max-Age**: Specifies the maximum time (in seconds) that a response can be cached. For example, `Cache-Control: max-age=3600` allows caching for one hour.
* **S-Max-Age**: Similar to max-age, but applies only to shared caches and overrides max-age. For example, `Cache-Control: s-maxage=3600` specifies that shared caches can cache the response for one hour.
* **No-Cache**: Allows caching but requires revalidation with the server before each use. For example, `Cache-Control: no-cache`.
* **No-Store**: Specifies that a response should not be stored in any cache, including browser caches and intermediary caches. For example, `Cache-Control: no-store`. Sensitive data should not be cached.

### Public Cache

Allows caching by any cache, including client-side and shared caches. Sensitive data should not be cached. For example, `Cache-Control: public`.

![public](https://i.ibb.co/G3DqJK4/image.png)

### Private Cache

Allows caching only by the client's browser and not by shared caches. Typically used for responses intended for a specific user. For example, `Cache-Control: private`.

![private](https://i.ibb.co/qRDYqSm/image.png)

### Etag Header

The Etag (Entity Tag) header is an HTTP response header that provides a mechanism for web servers to assign a unique identifier to a specific version of a resource. Etags are used for cache validation and conditional requests, allowing clients to determine whether their cached representation of a resource is still valid without having to download the entire resource again from the server.

The primary purpose of the Etag header is to provide a lightweight and efficient way to validate cached responses and reduce unnecessary data transfer

#### Steps

* **Etag Generation**: When a client creates a new resource server generates an Etag value for the current version of the resource.
* **Inclusion in Response**: The server includes the Etag value in response headers using the `Etag` header.
* **Storage by Client**: The client stores the response along with the Etag value.
* **Conditional Requests**: The client includes the stored Etag value in the `If-None-Match` header of subsequent requests. For example, `If-None-Match: "abcdef123456"`.
* **Validation by Server**: Upon receiving the request, server compares the Etag value provided by the client with the current Etag value of the resource. If the Etag values match, it indicates that the cached representation is still valid, and the server responds with a `304 Not Modified` status code, indicating that the client should use its cached copy. If the Etag values do not match, the server responds with the full resource content, along with a new Etag value for the updated version.

![Etag](https://i.ibb.co/WnZbYQM/Capture3.png)

## REST API Versioning

REST API versioning helps manage changes and updates to APIs while maintaining backward compatibility.

### Benefits

* Backward Compatibility
* Incremental Updates
* Flexibility
* Maintainability
* Documentation and Communication

### Examples of Breaking Changes

* Changing the URI structure of an existing API endpoint
	* Before: `GET /api/v1/product/{uid}`
	* After: `GET /api/v1/products/{uid}`
* This change could break existing client implementations that rely on the
old URI structure, causing requests to fail with `404 Not Found` errors.
* Modifying the behavior of an existing API methods or endpoints in a way that affects client applications.

### Versioning of Breaking Change

* **URL-Based**: Including the version number in the URL.
* **Header-Based**: Including the version number in request headers.

### Examples of Non-Breaking Changes

* Adding New Endpoints
* Optimizing Performance
* Adding New Fields

```py
# Before
{
	"id": "123456",
	"name": "Iphone 15"
}
# After
{
	"id": "123456",
	"name": "Iphone 15",
	"color": "grey"
}
```

### Best Practices for Handling API Changes

* Versioning Strategy
	* Choose URL or header-based versioning.
* API Documentation
* Backward Compatibility
* Graceful Deprecation
* Monitoring & Feedback

## REST API Specification

The REST API Specification ensures a clear and well-defined contract between the producer and consumer.  In the "REST API Specification," we follow two types of contracts:

* **Contract-Last (Code-First)**: Developing the API before creating the contract.
* **Contract-First (Design-First)**: Designing the API contract before implementation.

### Open API Specification

* [OpenAPI Specification v3.1.0](https://spec.openapis.org/oas/latest.html)
* [Swagger Editor](https://editor.swagger.io/)

## REST API Security

API security involves protecting APIs from various threats and ensuring safe interactions.

### Common API Threats and Vulnerabilities

* Poor security hygiene
* Authentication & Authorization Vulnerabilities
* Lack of Read and Write Granularity
* Failure to Implement Quotas and Throttling
* Improperly Set or Missing HTTP Headers
* Failure to Perform Input Validation, Sanitization, and Encoding in the method level.

### Securing REST API

* Authentication
	* Basic authentication
	* Session Authentication
	* JWT (JSON Web Tokens)
	* API Key
	* Oauth 2.0
		* Grant Types
			* Authorization Code
			* Implicit Grant
			* Authorization Code Grant with Proof Key for Code Exchange
			* Resource Owner Credentials
			* Client Credentials
			* Device Authorization Flow
			* Refresh Token Grant
* Authorization
* Input Validation and Sanitization
* Rate Limiting
* Security Headers
* Logging and Continuous Monitoring

### Access Control Best Practices

* Implement RBAC (Role-Based Access Control)
* Throttle Requests
* Use HTTPS
* Use UUIDs over Incremental IDs

### Input Data Validation

* Use Proper HTTP Methods
* Validate `Content-Type` in Request Headers
* Validate and Sanitize Request Body
* Avoid Sending Sensitive Data in Query Parameters
* Use Secure Server-Side Encryption

### Security Headers

* **Content-Security-Policy**: A powerful allow-list of what can happen on your page which mitigates many attacks Cross-Origin-Opener-Policy: Helps process-isolate your page
* **Cross-Origin-Resource-Policy**: Blocks others from loading your resources cross-origin
* **Origin-Agent-Cluster**: Changes process isolation to be origin-based
* **Referrer-Policy**: Controls the Referer header
* **Strict-Transport-Security**: Tells browsers to prefer HTTPS
* **X-Content-Type-Options**: Avoids MIME sniffing
* **X-DNS-Prefetch-Control**: Controls DNS prefetching
* **X-Download-Options**: Forces downloads to be saved (Internet Explorer only)
* **X-Frame-Options**: Legacy header that mitigates clickjacking attacks
* **X-Permitted-Cross-Domain-Policies**: Controls cross-domain behavior for Adobe products, like Acrobat
* **X-Powered-By**: Info about the web server. Remove because it could be used in simple attacks
* **X-XSS-Protection**: Legacy header that tries to mitigate XSS attacks, but makes things worse, so Helmet disables it

## API Management

API management is the organized control of APIs throughout their life cycle, including design, deployment, security, monitoring and monetization.

* Design
* Development
* Deployment
* API monitoring and analytics
* Documentation and Developer Portals
* Lifecycle Management
* Monetization & Billing

### Types of API Management Tools

* Proxy-Based
* Agent-Based
