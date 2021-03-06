# API User Guide

This document provides the guide to getting started on the MetalTrail platform and building integrations with the REST APIs exposed. The intended audience is a software developer well acquainted with REST APIs and JSON. With that knowledge, the Swagger API Documentation will be the best place to start testing and exploring the platform APIs.

## Swagger API Documentation

The REST APIs in MetalTrail follow the OpenAPI standard specifications. These API endpoints are documented using the famous [Swagger tool](https://swagger.io/), it allows developers to interact and execute API calls directly from the Swagger UI.

**Swagger URL -** [https://api-prod.metaltrail.com/api](https://api-prod.metaltrail.com/api)

## Authentication

As MetalTrail is an invite-only platform, authentication token is required to integrate with MetalTrail REST APIs. API calls without the authentication token will fail. 

Please visit [https://mavennet.com/solutions/metaltrail/](https://mavennet.com/solutions/metaltrail/) to begin the onboarding process.

## ProductsAPI

A product can be any steel-related product such as raw materials, heat, or semi-finished steel. The product information is stored in the form of a verifiable credential issued by the organization who created it.

Below are the set of functions that can be carried out on the product. Please be sure to have the authentication material at hand before beginning to work with the API endpoints.

#### Creation

The products can be created and fetched in the MetalTrail platform using below set of API endpoints. When a product is created, a corresponding creation of the product event is also stored in MetalTrail. Each product and the event is uniquely identified with a UUID.

```
POST /v1/products
GET /v1/products
GET /v1/products/{id}
```
#### Transformation

Within a lifecycle of the product(s), the (producer) owner of the product(s) can smelt, cast, finish, refine and fabricate while the downstream fabricator can finish, refine, or fabricate. Every new transformed child product issues a verifiable credential. Eg. Scrap steel and iron can be smelted into heat. Each transformation is stored as an event associated to the product.

```
POST /v1/products/transform
```

#### Ownership transfer

The ownership transfer of the product is carried out in three steps -
1. The current owner organization creates an ownership transfer request for the product with the prospective new owner of the product.
2. The prospective new owner receives the request, can fetch/review the information in the request and either accept it or request for change in the transfer request.
3. When the prospective new owner accepts the request, the transfer is executed automatically and a transfer of ownership event is created in MetalTrail.

```
POST /v1/products/transferownership/requests
PUT /v1/products/transferownership/requests
POST /v1/products/transferownership/confirm
POST /v1/products/transferownership
POST /v1/products/transfer/requests  
DELETE /v1/products/transfer/requests  
```

#### Custody transfer

The custody transfer of the product follows similar three steps process as ownership transfer - 
1. The current owner or the custodian organization creates a custody transfer request for the product with the prospective new custodian of the product.
2. The prospective new custodian receives the request, can fetch/review the information in the request and either accept it or request for change in the transfer request.
3. If the request is accepted by the prospective new custodian then the product will automatically be transferred to the new custodian. This will create a transfer of custody event which will be associated with the product. If the changes are requested then the current owner can make corresponding changes and start with step 2.

```
POST /v1/products/transfercustody/requests
PUT /v1/products/transfercustody/requests
POST /v1/products/transfercustody/confirm  
POST /v1/products/transfercustody
POST /v1/products/transfer/requests
DELETE /v1/products/transfer/requests
```

#### Transport

The custodian organization transports the product from one location to another. This movement is recorded in MetalTrail in the form of `start` and `end` transport events. Bill of Lading details are captured during this process. Transport events support correction of the details using the VC revocation standards. 

```
POST /v1/products/transport
PUT /v1/products/transport
```

#### Import Identifiers

The owner of the product should first share the product with their customs broker. The broker can then access the product and add relevant identifiers from the import declaration documents (i.e. entry summary ID, free trade zone admissions no. or QP In-Bond). This is stored as an event on MetalTrail. MetalTrail supports correction of the import identifier details using the VC revocation standards.

```
POST /v1/products/QPInBond  
PUT /v1/products/QPInBond  
```

#### Inspection

The owner or custodian organization can inspect the product for its physical and chemical specifications at any point of time. The inspection result is recorded as event in MetalTrail.

```
POST /v1/products/inspect  
```
#### Certify

MetalTrail enables users to submit pre-arrival and "intent to import" information to customs and regulatory agencies. The owner can claim free trade agreement benefits by "certifying" eligibility and submitting required data fields to the relevant parties. 

```
POST /v1/products/certify  
```
#### Share

The owner of the product can share or unshare the product with another organization(s). If the product is shared, an organization can access all the product details such as product's physical and chemical specifications, associated events with their location of occurrence, origin, product's composition/traceablility hierarchy and uploaded documents if any.

```
POST /v1/products/share  
GET /v1/products/share  
DELETE /v1/products/share  
```

## HTTP Status Codes & Errors

MetalTrail uses conventional HTTP response codes to indicate the success or failure of an API request. An exhaustive list for the same can be found [here](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status).

## Support

Please contact support at support@mavennet.com. 
