# EPP Samples - Host Mapping (RFC 5732)

This document provides the samples of EPP messages for all the EPP operations related to **hosts** defined in the RFC 5732.

---

# Table of Contents

- [1. Query Commands](#1-query-commands)
  - [Check](#check)
  - [Info](#info)
- [2. Transform Commands](#2-transform-commands)
  - [Create](#create)
  - [Delete](#delete)
  - [Update](#update)
- [3. Offline Review of a Request](#3-offline-review-of-a-request)


---

### 1. Query Commands

---

#### Check

Example **`<check>`** command:

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <command>
    <check>
      <host:check
        xmlns:host="urn:ietf:params:xml:ns:host-1.0">
        <host:name>ns1.example.com</host:name>
        <host:name>ns2.example.com</host:name>
        <host:name>ns3.example.com</host:name>
      </host:check>
    </check>
    <clTRID>ABC-12345</clTRID>
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
      <host:chkData
        xmlns:host="urn:ietf:params:xml:ns:host-1.0">
        <host:cd>
          <host:name avail="1">ns1.example.com</host:name>
        </host:cd>
        <host:cd>
          <host:name avail="0">ns2.example2.com</host:name>
          <host:reason>In use</host:reason>
        </host:cd>
        <host:cd>
          <host:name avail="1">ns3.example3.com</host:name>
        </host:cd>
      </host:chkData>
    </resData>
    <trID>
      <clTRID>ABC-12345</clTRID>
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
      <host:info
        xmlns:host="urn:ietf:params:xml:ns:host-1.0">
        <host:name>ns1.example.com</host:name>
      </host:info>
    </info>
    <clTRID>ABC-12345</clTRID>
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
      <host:infData
        xmlns:host="urn:ietf:params:xml:ns:host-1.0">
        <host:name>ns1.example.com</host:name>
        <host:roid>NS1_EXAMPLE1-REP</host:roid>
        <host:status s="linked" />
        <host:status s="clientUpdateProhibited" />
        <host:addr ip="v4">192.0.2.2</host:addr>
        <host:addr ip="v4">192.0.2.29</host:addr>
        <host:addr ip="v6">1080:0:0:0:8:800:200417A</host:addr>
        <host:clID>ClientY</host:clID>
        <host:crID>ClientX</host:crID>
        <host:crDate>1999-04-03T22:00:00.0Z</host:crDate>
        <host:upID>ClientX</host:upID>
        <host:upDate>1999-12-03T09:00:00.0Z</host:upDate>
        <host:trDate>2000-04-08T09:00:00.0Z</host:trDate>
      </host:infData>
    </resData>
    <trID>
      <clTRID>ABC-12345</clTRID>
      <svTRID>54322-XYZ</svTRID>
    </trID>
  </response>
</epp>
```

[↑ Back to Top](#table-of-contents)

---

### 2. Transform Commands

---

#### Create

Example **`<create>`** command:
```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <command>
    <create>
      <host:create
        xmlns:host="urn:ietf:params:xml:ns:host-1.0">
        <host:name>ns1.example.com</host:name>
        <host:addr ip="v4">192.0.2.2</host:addr>
        <host:addr ip="v4">192.0.2.29</host:addr>
        <host:addr ip="v6">1080:0:0:0:8:800:200417A</host:addr>
      </host:create>
    </create>
    <clTRID>ABC-12345</clTRID>
  </command>
</epp>
```

Example **`<create>`** response:
```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <response>
    <result code="1000">
      <msg>Command completed successfully</msg>
    </result>
    <resData>
      <host:creData
        xmlns:host="urn:ietf:params:xml:ns:host-1.0">
        <host:name>ns1.example.com</host:name>
        <host:crDate>1999-04-03T22:00:00.0Z</host:crDate>
      </host:creData>
    </resData>
    <trID>
      <clTRID>ABC-12345</clTRID>
      <svTRID>54322-XYZ</svTRID>
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
      <host:delete
        xmlns:host="urn:ietf:params:xml:ns:host-1.0">
        <host:name>ns1.example.com</host:name>
      </host:delete>
    </delete>
    <clTRID>ABC-12345</clTRID>
  </command>
</epp>
```

Example **`<delete>`** response:
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

#### Update

Example **`<update>`** command:
```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <command>
    <update>
      <host:update
        xmlns:host="urn:ietf:params:xml:ns:host-1.0">
        <host:name>ns1.example.com</host:name>
        <host:add>
          <host:addr ip="v4">192.0.2.22</host:addr>
          <host:status s="clientUpdateProhibited" />
        </host:add>
        <host:rem>
          <host:addr ip="v6">1080:0:0:0:8:800:200417A</host:addr>
        </host:rem>
        <host:chg>
          <host:name>ns2.example.com</host:name>
        </host:chg>
      </host:update>
    </update>
    <clTRID>ABC-12345</clTRID>
  </command>
</epp>
```

Example **`<update>`** response:
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

### 3. Offline Review of a Request

---

Examples describing a **`<create>`** command that requires offline review
   are included here.  Note the result code and message returned in
   response to the **`<create>`** command.
```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <response>
    <result code="1001">
      <msg>Command completed successfully; action pending</msg>
    </result>
    <resData>
      <host:creData
        xmlns:host="urn:ietf:params:xml:ns:host-1.0">
        <host:name>ns1.example.com</host:name>
        <host:crDate>1999-04-03T22:00:00.0Z</host:crDate>
      </host:creData>
    </resData>
    <trID>
      <clTRID>ABC-12345</clTRID>
      <svTRID>54322-XYZ</svTRID>
    </trID>
  </response>
</epp>
```

Example "review completed" service message:
```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <response>
    <result code="1301">
      <msg>Command completed successfully; ack to dequeue</msg>
    </result>
    <msgQ count="5" id="12345">
      <qDate>1999-04-04T22:01:00.0Z</qDate>
      <msg>Pending action completed successfully.</msg>
    </msgQ>
    <resData>
      <host:panData
        xmlns:host="urn:ietf:params:xml:ns:host-1.0">
        <host:name paResult="1">ns1.example.com</host:name>
        <host:paTRID>
          <clTRID>ABC-12345</clTRID>
          <svTRID>54322-XYZ</svTRID>
        </host:paTRID>
        <host:paDate>1999-04-04T22:00:00.0Z</host:paDate>
      </host:panData>
    </resData>
    <trID>
      <clTRID>BCD-23456</clTRID>
      <svTRID>65432-WXY</svTRID>
    </trID>
  </response>
</epp>
```

[↑ Back to Top](#table-of-contents)