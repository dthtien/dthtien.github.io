# REST - gRPC

## API
APIs are the set of rules and definitions that enable the applications to interect with each other.
API defines the methods and types of calls and requests one applicatin can make to another and formats those data and
recommendations

## REST
REST - Representational state transfer - is pupular set of design priciples for such HTTP based APIS

- Most populart architecture for building APIs
-  APIs that qualify as RESTful when it follow these priciples:
  - Communication is stateless - Each request is self contained and independent from other requests
  - Resources - Object that can be inspected and manipulated - are represented by URLs and
  - The state of a resource is updated by making a HTTP request with a standard method type such as POST or PUT to
    approprate URL

### Pros
- Caching easy - takes advantage of HTTP by eliminating the need for your client and sever to constantly intereact with
  each other, improving performance and scalabtity
- Simple
- Monitoring, Rate limiting easy
- Popular
### Cons
- Stateless
- Security - appropirate for public URLs, but it is not good for confidential data passage between client and server

### RPC - Remote procedure call
where code on one node appears to call a function on another node is called **Remote Procedure
Call - RPC**. The software that implements RPC is called and RPC framework or middleware - not all middleware is based
on RPC, there is also middleware that uses different communication models

Rest API
- resource oriented
- HTTP methods: GET, POST, PUT, PATCH, DELETE
- Provides flexibility for hardware architecture
- Support hypermedia and hyperlinks
- Allow to specify content types or accept header

RPC API
- Action oriented
- Support GET, POST
- No flexibilty in RPC for hardware architecture
- It does not supports hypermedia and hyperlinks
- REquire payloads of a few data types as XML for XML RPC
