# EPP Samples - Extensible Provisioning Protocol (RFC 5730)

This document provides the samples of EPP messages for all the EPP operations defined in the RFC 5730.

---

# Table of Contents

- [1. Service Discovery](#1-service-discovery)
  - [Hello](#hello)
  - [Greeting](#greeting)
- [2. Generics](#2-generics)
  - [Command](#command)
  - [Response](#response)
  - [Extension](#extension)
- [3. Session Management Commands](#3-session-management-commands)
  - [Login](#login)
  - [Logout](#logout)
- [4. Query Commands](#4-query-commands)
  - [Check](#check)
  - [Info](#info)
  - [Poll](#poll)
  - [Transfer (Query)](#transfer-query)
- [5. Object Transform Commands](#5-object-transform-commands)
  - [Create](#create)
  - [Delete](#delete)
  - [Renew](#renew)
  - [Transfer (Transform)](#transfer-transform)
  - [Update](#update)


---

### 1. Service Discovery 

---

#### Hello

Example **`<hello>`** :
```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <hello />
</epp>
```

[↑ Back to Top](#table-of-contents)

---

#### Greeting

Example **`<greeting>`** :
```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <greeting>
    <svID>Example EPP server epp.example.com</svID>
    <svDate>2000-06-08T22:00:00.0Z</svDate>
    <svcMenu>
      <version>1.0</version>
      <lang>en</lang>
      <lang>fr</lang>
      <objURI>urn:ietf:params:xml:ns:obj1</objURI>
      <objURI>urn:ietf:params:xml:ns:obj2</objURI>
      <objURI>urn:ietf:params:xml:ns:obj3</objURI>
      <svcExtension>
        <extURI>http://custom/obj1ext-1.0</extURI>
      </svcExtension>
    </svcMenu>
    <dcp>
      <access>
        <all />
      </access>
      <statement>
        <purpose>
          <admin />
          <prov />
        </purpose>
        <recipient>
          <ours />
          <public />
        </recipient>
        <retention>
          <stated />
        </retention>
      </statement>
    </dcp>
  </greeting>
</epp>
```

[↑ Back to Top](#table-of-contents)

---

### 2. Generics

---

#### Command

Example command with object-specified child elements:
```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <command>
    <info>
      <obj:info xmlns:obj="urn:ietf:params:xml:ns:obj">
        <obj:name>example</obj:name>
      </obj:info>
    </info>
    <clTRID>ABC-12345</clTRID>
  </command>
</epp>
```

[↑ Back to Top](#table-of-contents)

---

#### Response 

Example response without **`<value>`** or **`<resData>`**:
```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <response>
    <result code="1000">
      <msg lang="en">Command completed successfully</msg>
    </result>
    <trID>
      <clTRID>ABC-12345</clTRID>
      <svTRID>54321-XYZ</svTRID>
    </trID>
  </response>
</epp>
```

Example response with **`<resData>`**:
```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <response>
    <result code="1000">
      <msg>Command completed successfully</msg>
    </result>
    <resData>
      <obj:creData xmlns:obj="urn:ietf:params:xml:ns:obj">
        <obj:name>example</obj:name>
      </obj:creData>
    </resData>
    <trID>
      <clTRID>ABC-12345</clTRID>
      <svTRID>54321-XYZ</svTRID>
    </trID>
  </response>
</epp>
```

Example response with error value elements:
```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <response>
    <result code="2004">
      <msg>Parameter value range error</msg>
      <value xmlns:obj="urn:ietf:params:xml:ns:obj">
        <obj:elem1>2525</obj:elem1>
      </value>
    </result>
    <result code="2005">
      <msg>Parameter value syntax error</msg>
      <value xmlns:obj="urn:ietf:params:xml:ns:obj">
        <obj:elem2>ex(ample</obj:elem2>
      </value>
      <extValue>
        <value xmlns:obj="urn:ietf:params:xml:ns:obj">
          <obj:elem3>abc.ex(ample</obj:elem3>
        </value>
        <reason>Invalid character found.</reason>
      </extValue>
    </result>
    <trID>
      <clTRID>ABC-12345</clTRID>
      <svTRID>54321-XYZ</svTRID>
    </trID>
  </response>
</epp>
```

Example response with notice of waiting server messages: -->
```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <response>
    <result code="1000">
      <msg>Command completed successfully</msg>
    </result>
    <msgQ count="5" id="12345" />
    <trID>
      <clTRID>ABC-12345</clTRID>
      <svTRID>54321-XYZ</svTRID>
    </trID>
  </response>
</epp>
```

[↑ Back to Top](#table-of-contents)

---

#### Extension

For example, a protocol extension element would be described in generic terms as follows:
```xml
<epp>
  <extension>
    <!-- One or more extension elements. -->
    <ext:foo xmlns:ext="urn:ietf:params:xml:ns:ext">
      <!-- One or more extension child elements. -->
    </ext:foo>
  </extension>
</epp>
```

For example, the start of an object-specific command element would be described in generic terms as follows:
```xml
<EPPCommandName>
  <object:command xmlns:object="urn:ietf:params:xml:ns:object">
    <!-- One or more object-specific command elements. -->
  </object:command>
</EPPCommandName>
```
<!-- An object-specific response element would be described similarly: -->
```xml
<resData>
  <object:resData xmlns:object="urn:ietf:params:xml:ns:object">
    <!-- One or more object-specific response elements. -->
  </object:resData>
</resData>
```

A server-extended command element would be described in generic terms as follows:
```xml
<command>
  <!-- EPPCommandName can be "create", "update", etc. -->
  <EPPCommandName>
    <object:command xmlns:object="urn:ietf:params:xml:ns:object">
      <!-- One or more object-specific command elements. -->
    </object:command>
  </EPPCommandName>
  <extension>
    <!-- One or more server-defined elements. -->
  </extension>
</command>
```

A server-extended response element would be described similarly:
```xml
<response>
  <result code="1000">
    <msg lang="en">Command completed successfully</msg>
  </result>
  <extension>
    <!-- One or more server-defined elements. -->
  </extension>
  <trID>
    <clTRID>ABC-12345</clTRID>
    <svTRID>54321-XYZ</svTRID>
  </trID>
</response>
```

---

### 3. Session Management Commands

---

#### Login

Example **`<login>`** command:
```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <command>
    <login>
      <clID>ClientX</clID>
      <pw>foo-BAR2</pw>
      <newPW>bar-FOO2</newPW>
      <options>
        <version>1.0</version>
        <lang>en</lang>
      </options>
      <svcs>
        <objURI>urn:ietf:params:xml:ns:obj1</objURI>
        <objURI>urn:ietf:params:xml:ns:obj2</objURI>
        <objURI>urn:ietf:params:xml:ns:obj3</objURI>
        <svcExtension>
          <extURI>http://custom/obj1ext-1.0</extURI>
        </svcExtension>
      </svcs>
    </login>
    <clTRID>ABC-12345</clTRID>
  </command>
</epp>
```

Example **`<login>`** response:
```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <response>
    <result code="1000">
      <msg>Command completed successfully</msg>
    </result>
    <trID>
      <clTRID>ABC-12345</clTRID>
      <svTRID>54321-XYZ</svTRID>
    </trID>
  </response>
</epp>
```

[↑ Back to Top](#table-of-contents)

---

#### Logout

Example **`<logout>`** command:
```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <command>
    <logout />
    <clTRID>ABC-12345</clTRID>
  </command>
</epp>
```

Example **`<logout>`** response:
```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <response>
    <result code="1500">
      <msg>Command completed successfully; ending session</msg>
    </result>
    <trID>
      <clTRID>ABC-12345</clTRID>
      <svTRID>54321-XYZ</svTRID>
    </trID>
  </response>
</epp>
```

[↑ Back to Top](#table-of-contents)

---

### 4. Query Commands

---

#### Check

Example **`<check>`** command:
```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <command>
    <check>
      <obj:check xmlns:obj="urn:ietf:params:xml:ns:obj">
        <obj:name>example1</obj:name>
        <obj:name>example2</obj:name>
        <obj:name>example3</obj:name>
      </obj:check>
    </check>
    <clTRID>ABC-12346</clTRID>
  </command>
</epp>
```

Example **`<check>`** response:
```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <response>
    <result code="1000">
      <msg>Command completed successfully</msg>
    </result>
    <resData>
      <obj:chkData xmlns:obj="urn:ietf:params:xml:ns:obj">
        <obj:cd>
          <obj:name avail="1">example1</obj:name>
        </obj:cd>
        <obj:cd>
          <obj:name avail="0">example2</obj:name>
          <obj:reason>In use</obj:reason>
        </obj:cd>
        <obj:cd>
          <obj:name avail="1">example3</obj:name>
        </obj:cd>
      </obj:chkData>
    </resData>
    <trID>
      <clTRID>ABC-12346</clTRID>
      <svTRID>54322-XYZ</svTRID>
    </trID>
  </response>
</epp>
```

[↑ Back to Top](#table-of-contents)

---

#### Info

Example **`<info>`** command:
```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <command>
    <info>
      <obj:info xmlns:obj="urn:ietf:params:xml:ns:obj">
        <!-- Object-specific elements. -->
      </obj:info>
    </info>
    <clTRID>ABC-12346</clTRID>
  </command>
</epp>
```

Example **`<info>`** response:
```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <response>
    <result code="1000">
      <msg>Command completed successfully</msg>
    </result>
    <resData>
      <obj:infData xmlns:obj="urn:ietf:params:xml:ns:obj">
        <obj:roid>EXAMPLE1-REP</obj:roid>
        <!-- Object-specific elements. -->
      </obj:infData>
    </resData>
    <trID>
      <clTRID>ABC-12346</clTRID>
      <svTRID>54322-XYZ</svTRID>
    </trID>
  </response>
</epp>
```

[↑ Back to Top](#table-of-contents)

---

#### Poll

Example **`<poll>`** command:
```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <command>
    <poll op="req" />
    <clTRID>ABC-12345</clTRID>
  </command>
</epp>
```

Example **`<poll>`** response with object-specific information:
```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <response>
    <result code="1301">
      <msg>Command completed successfully; ack to dequeue</msg>
    </result>
    <msgQ count="5" id="12345">
      <qDate>2000-06-08T22:00:00.0Z</qDate>
      <msg>Transfer requested.</msg>
    </msgQ>
    <resData>
      <obj:trnData
        xmlns:obj="urn:ietf:params:xml:ns:obj-1.0">
        <obj:name>example.com</obj:name>
        <obj:trStatus>pending</obj:trStatus>
        <obj:reID>ClientX</obj:reID>
        <obj:reDate>2000-06-08T22:00:00.0Z</obj:reDate>
        <obj:acID>ClientY</obj:acID>
        <obj:acDate>2000-06-13T22:00:00.0Z</obj:acDate>
        <obj:exDate>2002-09-08T22:00:00.0Z</obj:exDate>
      </obj:trnData>
    </resData>
    <trID>
      <clTRID>ABC-12345</clTRID>
      <svTRID>54321-XYZ</svTRID>
    </trID>
  </response>
</epp>
```

Example **`<poll>`** acknowledgement command:
```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <command>
    <poll op="ack" msgID="12345" />
    <clTRID>ABC-12346</clTRID>
  </command>
</epp>
```

Example **`<poll>`** acknowledgement response:
```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <response>
    <result code="1000">
      <msg>Command completed successfully</msg>
    </result>
    <msgQ count="4" id="12345" />
    <trID>
      <clTRID>ABC-12346</clTRID>
      <svTRID>54322-XYZ</svTRID>
    </trID>
  </response>
</epp>
```

Example **`<poll>`** response with mixed message content and without object-specific information:
```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <response>
    <result code="1301">
      <msg>Command completed successfully; ack to dequeue</msg>
    </result>
    <msgQ count="4" id="12346">
      <qDate>2000-06-08T22:10:00.0Z</qDate>
      <msg lang="en">Credit balance low. <limit>100</limit><bal>5</bal>
      </msg>
    </msgQ>
    <trID>
      <clTRID>ABC-12346</clTRID>
      <svTRID>54321-XYZ</svTRID>
    </trID>
  </response>
</epp>
```

Example **`<poll>`** response to note an empty message queue:
```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <response>
    <result code="1300">
      <msg>Command completed successfully; no messages</msg>
    </result>
    <trID>
      <clTRID>ABC-12346</clTRID>
      <svTRID>54321-XYZ</svTRID>
    </trID>
  </response>
</epp>
```

[↑ Back to Top](#table-of-contents)

---

#### Transfer (Query)

Example **`<transfer>`** query command:
```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <command>
    <transfer op="query">
      <obj:transfer xmlns:obj="urn:ietf:params:xml:ns:obj">
        <!-- Object-specific elements. -->
      </obj:transfer>
    </transfer>
    <clTRID>ABC-12346</clTRID>
  </command>
</epp>
```

Example **`<transfer>`** query response:
```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <response>
    <result code="1000">
      <msg>Command completed successfully</msg>
    </result>
    <resData>
      <obj:trnData xmlns:obj="urn:ietf:params:xml:ns:obj">
        <obj:name>example</obj:name>
        <obj:trStatus>pending</obj:trStatus>
        <obj:reID>ClientX</obj:reID>
        <obj:reDate>2000-06-08T22:00:00.0Z</obj:reDate>
        <obj:acID>ClientY</obj:acID>
        <obj:acDate>2000-06-13T22:00:00.0Z</obj:acDate>
        <obj:exDate>2002-09-08T22:00:00.0Z</obj:exDate>
      </obj:trnData>
    </resData>
    <trID>
      <clTRID>ABC-12346</clTRID>
      <svTRID>54322-XYZ</svTRID>
    </trID>
  </response>
</epp>
```

[↑ Back to Top](#table-of-contents)

---

### 5. Object Transform Commands

---

#### Create

Example **`<create>`** command:
```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <command>
    <create>
      <obj:create xmlns:obj="urn:ietf:params:xml:ns:obj">
        <!-- Object-specific elements. -->
      </obj:create>
    </create>
    <clTRID>ABC-12345</clTRID>
  </command>
</epp>
```

Example **`<create>`** response with **`<resData>`**:
```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <response>
    <result code="1000">
      <msg>Command completed successfully</msg>
    </result>
    <resData>
      <obj:creData xmlns:obj="urn:ietf:params:xml:ns:obj">
        <!-- Object-specific elements. -->
      </obj:creData>
    </resData>
    <trID>
      <clTRID>ABC-12345</clTRID>
      <svTRID>54321-XYZ</svTRID>
    </trID>
  </response>
</epp>
```

[↑ Back to Top](#table-of-contents)

---

#### Delete

Example **`<delete>`** command:
```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <command>
    <delete>
      <obj:delete xmlns:obj="urn:ietf:params:xml:ns:obj">
        <!-- Object-specific elements. -->
      </obj:delete>
    </delete>
    <clTRID>ABC-12346</clTRID>
  </command>
</epp>
```

Example **`<delete>`** response without **`<resData>`**:
```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <response>
    <result code="1000">
      <msg>Command completed successfully</msg>
    </result>
    <trID>
      <clTRID>ABC-12346</clTRID>
      <svTRID>54322-XYZ</svTRID>
    </trID>
  </response>
</epp>
```

[↑ Back to Top](#table-of-contents)

---

#### Renew

Example **`<renew>`** command:
```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <command>
    <renew>
      <obj:renew xmlns:obj="urn:ietf:params:xml:ns:obj">
        <!-- Object-specific elements. -->
      </obj:renew>
    </renew>
    <clTRID>ABC-12346</clTRID>
  </command>
</epp>
```

Example **`<renew>`** response with **`<resData>`**:
```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <response>
    <result code="1000">
      <msg>Command completed successfully</msg>
    </result>
    <resData>
      <obj:renData xmlns:obj="urn:ietf:params:xml:ns:obj">
        <!-- Object-specific elements. -->
      </obj:renData>
    </resData>
    <trID>
      <clTRID>ABC-12346</clTRID>
      <svTRID>54322-XYZ</svTRID>
    </trID>
  </response>
</epp>
```

[↑ Back to Top](#table-of-contents)

---

#### Transfer (Transform)

Example **`<transfer>`** transform command:
```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <command>
    <transfer op="request">
      <obj:transfer xmlns:obj="urn:ietf:params:xml:ns:obj">
        <!-- Object-specific elements. -->
      </obj:transfer>
    </transfer>
    <clTRID>ABC-12346</clTRID>
  </command>
</epp>
```

Example **`<transfer>`** response with **`<resData>`**:
```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <response>
    <result code="1001">
      <msg>Command completed successfully; action pending</msg>
    </result>
    <resData>
      <obj:trnData xmlns:obj="urn:ietf:params:xml:ns:obj">
        <obj:name>example</obj:name>
        <obj:trStatus>pending</obj:trStatus>
        <obj:reID>ClientX</obj:reID>
        <obj:reDate>2000-06-08T22:00:00.0Z</obj:reDate>
        <obj:acID>ClientY</obj:acID>
        <obj:acDate>2000-06-13T22:00:00.0Z</obj:acDate>
        <obj:exDate>2002-09-08T22:00:00.0Z</obj:exDate>
      </obj:trnData>
    </resData>
    <trID>
      <clTRID>ABC-12346</clTRID>
      <svTRID>54322-XYZ</svTRID>
    </trID>
  </response>
</epp>
```

[↑ Back to Top](#table-of-contents)

---

#### Update

Example **`<update>`** command:
```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <command>
    <update>
      <obj:update xmlns:obj="urn:ietf:params:xml:ns:obj">
        <!-- Object-specific elements. -->
      </obj:update>
    </update>
    <clTRID>ABC-12346</clTRID>
  </command>
</epp>
```

Example **`<update>`** response without **`<resData>`**:
```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <response>
    <result code="1000">
      <msg>Command completed successfully</msg>
    </result>
    <trID>
      <clTRID>ABC-12346</clTRID>
      <svTRID>54322-XYZ</svTRID>
    </trID>
  </response>
</epp>
```

[↑ Back to Top](#table-of-contents)
