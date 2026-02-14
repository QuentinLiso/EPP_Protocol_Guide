# EPP Protocol - Communication between Registrars and Registries demystified

This guide is a user-friendly and practical yet in-depth presentation of the EPP (Extension Provisioning Protocol) defined by the following RFCs :

- [RFC 5730 - Extensible Provisioning Protocol (EPP)](https://datatracker.ietf.org/doc/html/rfc5730)
- [RFC 5731 - Extensible Provisioning Protocol (EPP) Domain Name Mapping](https://datatracker.ietf.org/doc/html/rfc5731)
- [RFC 5732 - Extensible Provisioning Protocol (EPP) Host Mapping](https://datatracker.ietf.org/doc/html/rfc5732)
- [RFC 5733 - Extensible Provisioning Protocol (EPP) Contact Mapping](https://datatracker.ietf.org/doc/html/rfc5733)
- [RFC 5734 - Extensible Provisioning Protocol (EPP) Transport over TCP](https://datatracker.ietf.org/doc/html/rfc5734)
- [RFC 3915 - Domain Registry Grace Period Mapping for the Extensible Provisioning Protocol (EPP)](https://datatracker.ietf.org/doc/html/rfc3915)

Through the presentation of the protocol's terms and syntax in this guide, you'll be able to understand how registrars and registries communicate and the workflow of the domain names management system.

Feel free to read the RFCs if you need more detailed explanations though, it's a lot of fun !


# Resources



# Table of Contents

- [1. Basics](#1-basics)
- [2. What EPP can do ?](#2-what-epp-can-do-)
  - [2.1. All EPP request have this first-level wrapper :](#21-all-epp-request-have-this-first-level-wrapper-)
  - [2.2. Inside this first-level wrapper, we use a set of predefined second-level tags depending on the EPP service we want to use :](#22-inside-this-first-level-wrapper-we-use-a-set-of-predefined-second-level-tags-depending-on-the-epp-service-we-want-to-use-)
- [3. EPP Services Deep Dive](#3-epp-services-deep-dive)
  - [3.1. XSD Sample Files](#31-xsd-sample-files)
      - [Step 1 :](#step-1-)
      - [Step 2 :](#step-2-)
        - [Step 3 :](#step-3-)
      - [Step 4 :](#step-4-)
      - [Step 5 :](#step-5-)
  - [3.2 EPP sample messages](#32-epp-sample-messages)
- [4. EPP Networking Implementation](#4-epp-networking-implementation)
  - [4.1. Connection Lifecycle (State Machine)](#41-connection-lifecycle-state-machine)
      - [Step 1 : Transport Establishment (TCP + TLS)](#step-1--transport-establishment-tcp--tls)
      - [Step 2 : The Server Greeting](#step-2--the-server-greeting)
      - [Step 3: Login \& Commands](#step-3-login--commands)
  - [4.2. Data Unit Format (Data Framing)](#42-data-unit-format-data-framing)
  - [4.3. Message Exchange Pattern](#43-message-exchange-pattern)
  - [4.4. Security Considerations](#44-security-considerations)
      - [Step 0. The Registry Root CA (Registry Setup Only)](#step-0-the-registry-root-ca-registry-setup-only)
      - [Step 1. The Server Certificate](#step-1-the-server-certificate)
      - [Step 2. The Client Certificate:](#step-2-the-client-certificate)
      - [Step 3. The Server Signature of the Client Certificate](#step-3-the-server-signature-of-the-client-certificate)
      - [Step 4. The TLS Handshake](#step-4-the-tls-handshake)



## 1. Basics

EPP (Extenstion Provisioning Protocol) is the protocol that registrars (e.g: OVH) and registries (e.g: AFNIC) use to communicate together to manipulate domain names and related elements (e.g: hosts, contacts).

Roughly speaking, the registrar sends an XML-formatted request to the registry and the registry sends an XML-formatted response back as per the request.

Below are an example of EPP request and its response. It looks daunting at first, but everything will become clear later.


Example of request from the registrar to the registry :

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <command>
    <info>
      <domain:info
       xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
        <domain:name hosts="all">example.com</domain:name>
        <domain:authInfo>
          <domain:pw>2fooBAR</domain:pw>
        </domain:authInfo>
      </domain:info>
    </info>
    <clTRID>ABC-12345</clTRID>
  </command>
</epp>
```

Example of response sent back by the registry to the registrar for the above request :

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
   <response>
      <result code="1000">
         <msg>Command completed successfully</msg>
      </result>
      <resData>
         <domain:infData
            xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
            <domain:name>example.com</domain:name>
            <domain:roid>EXAMPLE1-REP</domain:roid>
            <domain:status s="ok" />
            <domain:registrant>jd1234</domain:registrant>
            <domain:contact type="admin">sh8013</domain:contact>
            <domain:contact type="tech">sh8013</domain:contact>
            <domain:ns>
               <domain:hostObj>ns1.example.com</domain:hostObj>
               <domain:hostObj>ns1.example.net</domain:hostObj>
            </domain:ns>
            <domain:host>ns1.example.com</domain:host>
            <domain:host>ns2.example.com</domain:host>
            <domain:clID>ClientX</domain:clID>
            <domain:crID>ClientY</domain:crID>
            <domain:crDate>1999-04-03T22:00:00.0Z</domain:crDate>
            <domain:upID>ClientX</domain:upID>
            <domain:upDate>1999-12-03T09:00:00.0Z</domain:upDate>
            <domain:exDate>2005-04-03T22:00:00.0Z</domain:exDate>
            <domain:trDate>2000-04-08T09:00:00.0Z</domain:trDate>
            <domain:authInfo>
               <domain:pw>2fooBAR</domain:pw>
            </domain:authInfo>
         </domain:infData>
      </resData>
      <trID>
         <clTRID>ABC-12345</clTRID>
         <svTRID>54322-XYZ</svTRID>
      </trID>
   </response>
</epp>
```


## 2. What EPP can do ?

Loosely translated, EPP is :

- an XML-formatted exchange between a registrar and a registry
- performing CRUD operations over **objects**
- that are stored in a registry's databases

**Objects** is a generic term used in the RFC to make sure that the protocol stays polymorphic and can be applied to several objects : **domains, hosts, contacts** and even **custom objects** defined and implemented by the registry.

Since EPP is an XML-based protocol, each request/response uses specific tags and attributes to access these services. In this section, we'll stick to the very basic tags to :

- Understand what are the services the EPP protocol offers
- Understand the structure of an EPP message to offer those services

(Note : A basic understanding of the XML syntax is helpful. We'll do our best to explain the XML-specific elements used by EPP though)



### 2.1. All EPP request have this first-level wrapper :

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
 
    <!-- ... -->
 
</epp>
```

Line 1 : identifies that the XML protocol is used (standard XML header)

Line 2 : identifies that the EPP protocol is used with the <epp></epp> tag. xmlns stands for XML Namespace.

→ It means that all the tags used in the EPP message are defined in the namespace called "urn:ietf:params:xml:ns:epp-1.0".

→ The namespace "urn:ietf:params:xml:ns:epp-1.0" itself is defined in an XSD file, which is a file that defines what XML tags, attributes, types, etc. are allowed in a specific XML file.

→ E.g :
- the XSD file creates the blueprint for the <command> tag, and encapsulates it in the XML namespace "urn:ietf:params:xml:ns:epp-1.0".
- Then, the XML message needs to add the attribute xmlns="urn:ietf:params:xml:ns:epp-1.0" to the <epp> tag to use the <command> tag as defined in the XSD file

Again don't worry if it's not clear yet : section 3.1 below is all about XSD and namespaces



### 2.2. Inside this first-level wrapper, we use a set of predefined second-level tags depending on the EPP service we want to use :

- Service 1 : Service Discovery 
  - `<hello />`
  - `<greetings></greetings>`
- Service 2 : Commands
  - `<command></command>`
- Service 3 : Response
  - `<response></response>`
- Service 4 : Extension
  - `<extension></extension>`

Each EPP message can have one and only one of the above tags :

- If the registrar sends an EPP message with <hello />, the registry replies with <greetings></greetings>
- If the registrar sends an EPP message with <command></command>, the registry replies with <response></response>
- Extension is a special case we'll cover later (TODO)

Below are the sample XML blocks using these tags. Don't worry about the blank `<!-- ... -->` lines, we'll come to that in the next sections.

**hello tag**
```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
    <hello />
</epp>
```

**greetings tag**
```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
    <greetings>
         
        <!-- ... --> 
 
    </greetings>
</epp>
```

**command tag**
```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
    <command>
         
        <!-- ... --> 
 
    </command>
</epp>
```

**response tag**
```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
    <response>
         
        <!-- ... --> 
 
    </response>
</epp>
```

**extension tag**
```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
    <extension>
         
        <!-- ... --> 
 
    </extension>
</epp>
```


## 3. EPP Services Deep Dive

As per the previous part, we have 2 indent levels :

- Level 1 : xml && epp tags
- Level 2 : hello | greetings | command | response | extension tag


Now, we need to populate the level 2 with child XML elements. Let's call them Level 3+ elements (because each child can have a sub-child tag, a sub-sub-child tag, etc.)

Which means we face 2 challenges :

- What are the available Level 3+ elements for each Level 2 as per the RFC ?
- What each Level 3+ element mean, when to use them, and which ones are mandatory/optional ?


To handle this as clearly as possible, we will :

- Rely upon the XSD sample files provided by the RFCs for comprehensiveness of allowed XML elements.
- Provide the EPP sample messages from the RFCs for concrete enforcement of the previous points.



### 3.1. XSD Sample Files

The XSD files from the RFCs are provided in the **xsd-annotated** and **xsd-official** folders. The annotated files are just extensions of the official files, with additional information about the use cases of each element.

You can download them as reference and open them in your code editor (it helps A LOT to keep track of each type definition lol)

| RFC | Official XSD | Annotated XSD | XML Namespace defined
|:-:|:-:|:-:|:-:|
|Base EPP (RFC 5730) | [File](./xsd-rfc-official/1_epp.xsd) | [File](./xsd-rfc-annotated/1_epp_annotated.xsd) | urn:ietf:params:xml:ns:epp-1.0 |
|Miscellaneous Types (RFC 5730) | [File](./xsd-rfc-official/0_eppcom.xsd) | [File](./xsd-rfc-annotated/0_eppcom_annotated.xsd) | urn:ietf:params:xml:ns:eppcom-1.0 |
|Domains (RFC 5731) | [File](./xsd-rfc-official/2_domain.xsd) | [File](./xsd-rfc-annotated/2_domain_annotated.xsd) | urn:ietf:params:xml:ns:domain-1.0 |
|Hosts (RFC 5732) | [File](./xsd-rfc-official/3_host.xsd) | [File](./xsd-rfc-annotated/3_host_annotated.xsd) | urn:ietf:params:xml:ns:host-1.0 |
|Contacts (RFC 5733) | [File](./xsd-rfc-official/4_contact.xsd) | [File](./xsd-rfc-annotated/4_contact_annotated.xsd) | urn:ietf:params:xml:ns:contact-1.0 |
|Redemption Grade Period for Domains (RFC 3915) | [File](./xsd-rfc-official/5_redemptionGracePeriod.xsd) | [File](./xsd-rfc-annotated/5_redemptionGracePeriod.xsd) | urn:ietf:params:xml:ns:rgp-1.0 |

An XSD file is simply a file, written in XML defining the structure of a target XML file. For example, it defines what are the tags nested inside the `<command></command>` (with additional relevant information about these nested tags).

As hard as it might be to read the XSD files, it's the best way to have a single source of truth because the XSD file indicates :

- The expected child elements of a tag
- The expected attributes of a tag
- The data type to provide to a tag or an attribute
- If an element is mandatory, optional, a choice list or an enumeration
- The target namespace that encapsulates the tags

A good quick guide about XSD files is available here : [XSD Guide](https://www.w3schools.com/xml/schema_intro.asp)

To stay in the context of EPP, we'll just follow a simple example from the Base EPP XSD.

##### Step 1 :

```xml
<schema targetNamespace="urn:ietf:params:xml:ns:epp-1.0"
    xmlns:epp="urn:ietf:params:xml:ns:epp-1.0"
    xmlns:eppcom="urn:ietf:params:xml:ns:eppcom-1.0"
    xmlns="http://www.w3.org/2001/XMLSchema"
    elementFormDefault="qualified">
```

This means that :

- This XSD file defines the namespace "urn:ietf:params:xml:ns:epp-1.0"
- The actual **`<epp>`** tag in the EPP message must include the namespace : **`<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">`**
- Each type prefixed with "epp:" in the XSD file belongs to this namespace and is defined elsewhere in this XSD file
- Each type prefixed with "eppcom:" belongs to the namespace "urn:ietf:params:xml:ns:eppcom-1.0" defined in the file 0_eppcom.xsd

##### Step 2 :

```xml
<!--
Every EPP XML instance must begin with this element.
-->
 <element name="epp" type="epp:eppType" />
```

This means that the whole EPP message will be enclosed in an **`<epp xmlns="urn:ietf:params:xml:ns:epp-1.0"></epp>`** tag

###### Step 3 :

We follow the cascade definition to epp:eppType :

```xml
<!--
An EPP XML instance must contain a greeting, hello, command,
response, or extension.
-->
 <complexType name="eppType">
     <choice>
         <element name="greeting" type="epp:greetingType" />
         <element name="hello" />
         <element name="command" type="epp:commandType" />
         <element name="response" type="epp:responseType" />
         <element name="extension" type="epp:extAnyType" />
     </choice>
 </complexType>
```

This means that :

- the epp tag encloses nested types (complexType)
- the nested type is one of the following elements (choice) : **`<greeting></greeting>`**, **`<hello />`**, **`<command></command>`**, **`<response></response>`** or **`<extension></extension>`**

##### Step 4 :
     
We follow the cascade definition to epp:commandType :

```xml
<!--
Command types.
-->
 <complexType name="commandType">
     <sequence>
         <choice>
             <element name="check" type="epp:readWriteType" />
             <element name="create" type="epp:readWriteType" />
             <element name="delete" type="epp:readWriteType" />
             <element name="info" type="epp:readWriteType" />
             <element name="login" type="epp:loginType" />
             <element name="logout" />
             <element name="poll" type="epp:pollType" />
             <element name="renew" type="epp:readWriteType" />
             <element name="transfer" type="epp:transferType" />
             <element name="update" type="epp:readWriteType" />
         </choice>
         <element name="extension" type="epp:extAnyType"
             minOccurs="0" />
 
         <element name="clTRID" type="epp:trIDStringType"
             minOccurs="0" />
     </sequence>
 </complexType>
 ```

This means that the **`<command>`** tag encloses theses 3 tags (sequence) : 
- One of the **`<check>`**, **`<create>`**, ..., **`<update>`** tag
- One optional (minOccurs="0") **`<extension>`** tag
- One optional (minOccurs="0") **`<clTRID>`** tag

##### Step 5 : 

We follow to epp:readWriteType which is a bit trickier :

```xml
<!--
All other object-centric commands.  EPP doesn't specify the syntax or
semantics of object-centric command elements.  The elements MUST be
described in detail in another schema specific to the object.
-->
 <complexType name="readWriteType">
     <sequence>
         <any namespace="##other" />
     </sequence>
 </complexType>
```

It means that the child elements of the tag which are of type epp:readWriteType (check, create, delete, renew...) MUST and will be defined in any other namespace.
→ They are implemented in the XSD file defining the namespace for the object (host, domain, contact).

For example, the syntax of the EPP check command for the domain object (defined in RFC 5731) will be :

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <command>
    <check>
 
      <domain:check
       xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
        <!-- ... -->
      </domain:check>
 
    </check>
    <clTRID>ABC-12345</clTRID>
  </command>
</epp>
```

And finally, to add the remaining nested tags required for domain:check, we refer to the check definition of domains XSD file with the same logic.

We will not keep going because this can be long, but you get the point : **follow the type definitions throught the XSD file, and refer to the XSD documentation if needed.**



### 3.2 EPP sample messages

To follow the XSD file, examples of EPP messages are provided by the RFCs. We compiled them in this repository.
The EPP sample messages from the RFCs are provided in the **epp-samples** folder :

| RFC | Markdown Format | XML Format
|-|:-:|:-:|
|Base EPP (RFC 5730) | [Markdown](./epp-samples/md/samples-rfc5730-epp.md) | [XML](./epp-samples/xml/samples-rfc5730-epp.xml)
|Domains (RFC 5731) | [Markdown](./epp-samples/md/samples-rfc5731-epp.md) | [XML](./epp-samples/xml/samples-rfc5731-epp.xml)
|Hosts (RFC 5732) | [Markdown](./epp-samples/md/samples-rfc5732-epp.md) | [XML](./epp-samples/xml/samples-rfc5732-epp.xml)
|Contacts (RFC 5733) | [Markdown](./epp-samples/md/samples-rfc5733-epp.md) | [XML](./epp-samples/xml/samples-rfc5733-epp.xml)
|Redemption Grade Period for Domains (RFC 3915) | [Markdown](./epp-samples/md/samples-rfc3915-epp.md) | [XML](./epp-samples/xml/samples-rfc3915-epp.xml)



## 4. EPP Networking Implementation

The [RFC 5734](https://datatracker.ietf.org/doc/html/rfc5734#autoid-3) defines how the Extensible Provisioning Protocol (EPP) maps onto a TCP/TLS connection. For a registrar/registry interface, it defines the:
- State Machine
- Data Framing
- Message Exchange Pattern
- Security Considerations 



### 4.1. Connection Lifecycle (State Machine)

The protocol follows a very strict handshake and session sequence :

##### Step 1 : Transport Establishment (TCP + TLS)

- The client initiates a TCP connection to the server (Standard Port assigned by IANA : 700, [RFC 5734 - Section 7](https://datatracker.ietf.org/doc/html/rfc5734#section-7)).
- TLS is mandatory. A TLS handshake must be completed immediately after the TCP connection is established. The RFC requires TLS to protect the XML payload.

##### Step 2 : The Server Greeting

- Once the secure tunnel is up, the Server MUST send a **`<greeting>`** (see [RFC 5730](https://datatracker.ietf.org/doc/html/rfc5730#section-2.4) and [XSD file](./xsd-rfc-annotated/1_epp_annotated.xsd)).
- The client must wait for and parse this greeting before sending any data.

##### Step 3: Login & Commands

- The client sends a **`<login>`** command to authenticate.
- The session remains active until a **`<logout>`** command is sent or the connection is terminated due to timeout/error.
- Timeouts: The EPP server should implement a timeout. If a client connects but doesn't send a command within a specific window, the server will close the connection.
- Idempotency: EPP commands are generally idempotent. If the connection drops after you send a command but before you get the response, you should be able to retry (though you'd typically check the object state first).



### 4.2. Data Unit Format (Data Framing)

TCP is a stream-oriented protocol, meaning it doesn't know where one XML message ends and another begins. Each EPP message must include a 4-bytes header describing the total length of the message, including those 4-bytes

Steps:
1. **Read Header**: Read exactly 4 bytes from the socket.
2. **Calculate Payload**: XML_Length = Header_Value - 4.
3. **Read Payload**: Read XML_Length bytes to get the full XML instance.
4. **Parsing**: Pass the resulting buffer to the XML message parser.



### 4.3. Message Exchange Pattern

- EPP follows a synchronous Command-Response model.
- Pipelining: A client may send multiple commands before receiving a response. However, the server must process them independently and return responses in the same order they were received.
- One-to-One Mapping: Each EPP data unit must contain exactly one EPP message.



### 4.4. Security Considerations

Unlike a standard website where only the server has a certificate, EPP requires the client to have a certificate as well (Mutual TLS).

##### Step 0. The Registry Root CA (Registry Setup Only)

- Before any certificates can be issued, the Registry must create its own internal Certificate Authority (CA).

```bash
$ openssl genrsa -out ca-key.key 4096
```
- Output: **ca-key.key** (The "Master" Private Key)

```bash
$ openssl req -x509 -new -nodes -key ca-key.key -sha256 -days 3650 -out ca-cert.pem -subj "/CN=MyLocalRegistryCA"
```
- Output: **ca-cert.pem** (The Public Root Certificate).

- The Registry uses these files to sign all incoming Registrar requests.

##### Step 1. The Server Certificate

  - Generated by the Registry when setting up the EPP Server :

```bash
$ openssl req -nodes -new -sha256 -newkey rsa:4096 -keyout server.key -out server.csr -subj /countryName="FR" /commonName="name"
```
- Output: **server.key** (Private) and **server.csr** (Public)
- In production, the **server.csr** is signed by a Public CA (Certificated Authrotiy) (e.g. DigiCert, Let's Encrypt) to produce a **server.crt**.
- In local development, you use `mkcert` to generate a trusted **server.crt** for localhost, or act as your own CA.


##### Step 2. The Client Certificate:

- The Registrar generates a Private Key and a Certificate Signing Request (CSR).

```bash

$ openssl req -nodes -new -sha256 -newkey rsa:4096 -keyout name.key -out name.csr -subj /countryName="FR" commonName="requiredName"
```
- Output: **name.key** (Private) and **name.csr** (Public)

- The Registrar sends **name.csr** to the Registry via their administrative portal.

##### Step 3. The Server Signature of the Client Certificate
- The Registry signs **name.csr** using their own Internal Root CA **ca-cert.pem** and **ca-key.key**. 

```bash
$ openssl x509 -req -in name.csr -CA ca-cert.pem -CAkey ca-key.key -CAcreateserial -out signed.pem -days 365
```

- Output: **signed.pem**

- Registry sends back a **signed.pem** to Registrar.

- The Registry now "knows" this certificate. During connection, it will verify that the client certificate was signed by the Registry's own **ca-cert.pem**.
  
- Note: Many Registries use IP Whitelisting in addition to certificates.

##### Step 4. The TLS Handshake

When you open that socket on Port 700, the handshake occurs before a single byte of EPP XML is exchanged.

1. **The Hello Phase**

    - **Client Hello:** Client sends supported cipher suites and a random string.
    - **Server Hello:** Server picks the cipher suite and sends its random string.
    - **Server Certificate:** The Registry sends its public certificate (**server.crt**).
    - **Certificate Request:** The Registry requests the client certificate to identify the Registrar.


2. **The Authentication Phase**

   - **Client Certificate:** The Registrar sends the signed certificate (**signed.pem**).
   - **Client Key Exchange:** Parameters are exchanged to calculate the "Pre-Master Secret."
   - **Certificate Verify:** The client sends a digital signature created using **name.key**. This proves the client owns the private key associated with **signed.pem**.

3. **The Secure Tunnel**
   - **Finished:** Both sides confirm encryption is active.
   - **The Greeting:** The Registry finally sends the EPP **`<greeting>`** XML over the encrypted tunnel.
