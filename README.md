# SOAP API

<br/>

## Table of Contents
- [Introduction](#introduction)
- [Core Concepts](#core-concepts)
- [Structure of a SOAP Message](#structure-of-soap-message)
- [Request Response Cycle)](#request-response-cycle)
- [WSDL](#wsdl)
- [Features](#features)
- [Use Cases](#use-cases)
- [Pros and Cons](#pros-and-cons)
- [Request and Response](#request-and-response)
- [Summary](#summary)
- [Create SOAP API](#create-soap-api)

<br/><br/>

## Introduction
- `SOAP` stands for `Simple Object Access Protocol`. It's a protocol used to `exchange structured information` in web services using `XML`
- It was developed by `Microsoft` and later became a `W3C standard` (`World Wide Web Consortium`)

<br/><br/>

## Core Concepts

### Protocol
- SOAP is a protocol (not just an architecture like REST)
- It defines a `strict set of rules` for how messages should be formatted and transmitted

### XML-based
- All messages are formatted in `XML`, making them both `human-readable` and `machine-readable` but `more verbose` than JSON

### Transport Agnostic
- SOAP can work over multiple transport protocols:
    - HTTP (most common)
    - SMTP
    - TCP, etc.

### Strict Messaging Format
- SOAP enforces a `strict message format` that includes headers and a body, often used in enterprise environments for security and reliability

<br/><br/>

## Structure of a SOAP Message
- A typical SOAP message looks like this

```xml
<soap:Envelope xmlns:soap="http://www.w3.org/2003/05/soap-envelope">
  <soap:Header>
    <!-- Optional: metadata like authentication -->
  </soap:Header>
  <soap:Body>
    <m:GetUserDetails xmlns:m="http://example.com/user">
      <m:UserId>123</m:UserId>
    </m:GetUserDetails>
  </soap:Body>
</soap:Envelope>
```

### Components of SOAP Message
- **Envelope**: Root element. Defines it as a SOAP message.
- **Header**: Optional metadata (authentication, security, etc.)
- **Body**: Contains the actual request/response data.
- **Fault**: Error information (if any).

<br/><br/>

## Request Response Cycle
- Client sends a SOAP request (in XML) over HTTP
- Server receives and processes the request
- Server sends a SOAP response (also in XML)
- Response contains either:
    - Data
    - Or a fault message (in case of errors)

### Conceptual Output

```cmd
<definitions>
├── <message>        --> Define inputs/outputs
├── <portType>       --> Declare operations (methods)
├── <binding>        --> Define how to send the request
└── <service>        --> Publish endpoint location
```

<br/><br/>

## WSDL
- `SOAP Services` are described using `WSDL`, an XML document that acts like a contract between client and server.

- It includes:
    - `Types`: Data types used
    - `Messages`: Request and response formats
    - `Port Types`: Abstract operations (like functions)
    - `Bindings`: Protocols and data format
    - `Service`: URL where service is hosted

- 💡 WSDL allows clients to auto-generate code (stubs) to consume services

- WSDL is an XML document that describes how a SOAP web service works. 
- It defines:
    - The operations available (like createPost)
    - The messages used in each operation
    - How messages are bound to a protocol (SOAP/HTTP)
    - Where the service is located (endpoint URL)

### 🧩 1. Messages — What data is exchanged
- Defines the input and output structure for operations
```xml
<message name="createPostRequest">
  <part name="title" type="xsd:string"/>
  <part name="content" type="xsd:string"/>
  <part name="userId" type="xsd:string"/>
</message>

<message name="createPostResponse">
  <part name="postId" type="xsd:string"/>
</message>
```

- `createPostRequest`: takes `title`, `content`, `userId`
- `createPostResponse`: returns the newly created `postId`

These are like parameters for your function and return values

### 🧠 2. Port Type — The contract (methods)
- Defines the available operations in the service
```xml
<portType name="PostPortType">
  <operation name="createPost">
    <input message="tns:createPostRequest"/>
    <output message="tns:createPostResponse"/>
  </operation>
</portType>
```
- This says: there's an operation `createPost` that expects the `createPostRequest` message and returns a `createPostResponse`.
- This is like an interface in programming: what the service can do.

### 🧷 3. Binding — How the methods are called
- Defines how to map the operations to SOAP (or another protocol like HTTP)
```xml
<binding name="PostBinding" type="tns:PostPortType">
  <soap:binding style="document" transport="http://schemas.xmlsoap.org/soap/http"/>
  <operation name="createPost">
    <soap:operation soapAction="createPost"/>
    <input><soap:body use="literal"/></input>
    <output><soap:body use="literal"/></output>
  </operation>
</binding>
```
- Binds PostPortType operations to SOAP over HTTP.
- style="document" means it's a full XML document (not just parameters).
- soapAction="createPost" is used by some SOAP clients to identify the method.
- This is like defining how your function can be called over the network.

### 📍 4. Service — Where the service is
- Defines the endpoint URL and which binding to use
```xml
<service name="PostService">
  <port name="PostPortType" binding="tns:PostBinding">
    <soap:address location="http://localhost:3000/soap/post"/>
  </port>
</service>
```
- Tells the client: go to this URL to use PostPortType via PostBinding
- This is like registering your service with a known address

### 🔄 Request Flow Summary
1. 🧑‍💻 Client reads WSDL:
    - Sees the operation createPost, its inputs/outputs.
    - Prepares a SOAP XML matching the createPostRequest.
2. 🌐 Client sends `POST` to `http://localhost:3000/soap/post`  
3. 🧠 Server:
    - Matches the request with the WSDL-defined createPost operation.
    - Extracts title, content, userId from the XML.
    - Calls your Node.js service function createPost({ title, content, userId }).
4. 📨 Response:
    - Server wraps the result (postId) in a SOAP response.
    - Sends it back according to createPostResponse structure. 

### 📌 Visual Map
```cmd
WSDL
├── messages (input/output structure)
├── portType (defines methods)
├── binding (how methods are mapped to SOAP)
└── service (where it's available)
```

<br/><br/>

## Features
| Feature                  | Description                                      |
| ------------------------ | ------------------------------------------------ |
| **WS-Security**          | Handles encryption, tokens, signatures           |
| **WS-ReliableMessaging** | Ensures delivery even in case of network failure |
| **ACID transactions**    | Supports complex transaction management          |
| **Stateful operations**  | Supports sessions (vs. REST’s stateless nature)  |

<br/><br/>

## Use Cases
- Banking/Finance APIs: Strict security and transactions
- Telecom systems
- Enterprise systems: ERP, CRM
- Payment gateways: Some still use SOAP (e.g., older PayPal APIs)

<br/><br/>

## Pros and Cons

### ✅ Pros
- Strong security (WS-Security)
- Strict contracts (WSDL)
- Language-neutral
- Good for complex and transactional operations

### ❌ Cons
- Verbose XML format
- Slower due to parsing XML
- Harder to test and debug
- Less suitable for web/mobile apps

<br/><br/>

## Request and Response

### 🔍 Request
```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:ser="http://service.example.com/">
   <soapenv:Header/>
   <soapenv:Body>
      <ser:getUser>
         <id>123</id>
      </ser:getUser>
   </soapenv:Body>
</soapenv:Envelope>
```

### 📩 Response
```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/">
   <soapenv:Body>
      <getUserResponse>
         <name>John Doe</name>
         <email>john@example.com</email>
      </getUserResponse>
   </soapenv:Body>
</soapenv:Envelope>
```

<br/><br/>

## Summary
| Concept         | Description                               |
| --------------- | ----------------------------------------- |
| SOAP            | XML-based protocol for web services       |
| Envelope        | Root element in SOAP message              |
| WSDL            | Describes the service interface           |
| WS-\* standards | Set of extensions (security, reliability) |
| Used in         | Banking, telecom, enterprise apps         |
| Tools           | SoapUI, Postman, wsimport, JAX-WS, etc.   |

<br/><br/>

## Create SOAP API
- Visit [this]() repo and access the source code and read how we can create a SOAP API

<br/><br/>