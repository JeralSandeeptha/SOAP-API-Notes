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