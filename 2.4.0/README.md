# Open Networks API 2.4.0

This API aims to provide the necessary functionality to enable automation and communication between service providers (SP) 
and open access network communication operators (CO). Enabling service providers to deliver services to the end customer in a secure 
and efficient way without transmitting unnecessary personal information. 

The API exposes the available network accesses and enables the SP to manage the last mile delivery in open networks.

Some of the properties of this API includes: 

 * Provide the SP with available accesses from the CO, accesses include
   * Address
   * Network outlet
   * Deliverable services
   * Customer placed equipment
 * Manageability of deliverable services
 * Secure communications
 * GDPR safe operations
 * Support for automated processes

## Table of contents

* [Changelog](changelog.md)
* [Accesses](spec/accesses.md)
* [Data formats](common/dataformats.md)
* [Endpoints](spec/endpoints.md)
* [Responses](common/responses.md)
* [Option82 lookup](spec/option82_lookup.md)
* [Order](spec/orders.md)
* [Order events](spec/orderevents.md)
* [SP contacts](spec/contacts.md)
* [Subscriptions - list active services](spec/subscriptions.md)
* [Web portal - Forward customers to SP service portals](spec/web_portal.md)
* [Tickets](spec/tickets.md)
* [Technical information](spec/technical_info.md)
 
## General guidelines

Open Network API is a REST/JSON API and must be exposed through a secure HTTP/TLS connection.

Authentication is handled with the basic authentication scheme. (see [RFC 7617](https://tools.ietf.org/html/rfc7617))


The API URL must contain the API version number. The API subset must use the documented path in the following format.
/[api version number]/[api endpoint]

Examples: 
   * https://server.com/onapi/2.4/accesses/
   * https://server.com/2.4/accesses/
   * https://server.com/some/longer/path/2.4/accesses/