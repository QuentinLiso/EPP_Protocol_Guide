# EPP Samples - Domain Registry Grace Period Mapping (RFC 3915)

This document provides the samples of EPP messages for all the EPP operations regarding domain names subject to **"grace period" policies** defined in the RFC 3915.

---

# Table of Contents

- [1. Query Commands](#1-query-commands)
  - [Info](#info)
- [2. Transform Commands](#2-transform-commands)
  - [Update](#update)


---

### 1. Query Commands

---

#### Info

Example **`<info>`** response for "addPeriod" status:
```xml
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
  <response>
    <result code="1000">
      <msg>Command completed successfully</msg>
    </result>
    <resData>
      <domain:infData
        xmlns:domain="urn:ietf:params:xml:ns:domain-1.0"
        xsi:schemaLocation="urn:ietf:params:xml:ns:domain-1.0 domain-1.0.xsd">
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
        <domain:crID>ClientX</domain:crID>
        <domain:crDate>2003-11-26T22:00:00.0Z</domain:crDate>
        <domain:exDate>2005-11-26T22:00:00.0Z</domain:exDate>
        <domain:authInfo>
          <domain:pw>2fooBAR</domain:pw>
        </domain:authInfo>
      </domain:infData>
    </resData>
    <extension>
      <rgp:infData xmlns:rgp="urn:ietf:params:xml:ns:rgp-1.0"
        xsi:schemaLocation="urn:ietf:params:xml:ns:rgp-1.0 rgp-1.0.xsd">
        <rgp:rgpStatus s="addPeriod" />
      </rgp:infData>
    </extension>
    <trID>
      <clTRID>ABC-12345</clTRID>
      <svTRID>54322-XYZ</svTRID>
    </trID>
  </response>
</epp>
```

Example **`<info>`** response for "redemptionPeriod" status:
```xml
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
  <response>
    <result code="1000">
      <msg>Command completed successfully</msg>
    </result>
    <resData>
      <domain:infData
        xmlns:domain="urn:ietf:params:xml:ns:domain-1.0"
        xsi:schemaLocation="urn:ietf:params:xml:ns:domain-1.0 domain-1.0.xsd">
        <domain:name>example.com</domain:name>
        <domain:roid>EXAMPLE1-REP</domain:roid>
        <domain:status s="pendingDelete" />
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
    <extension>
      <rgp:infData xmlns:rgp="urn:ietf:params:xml:ns:rgp-1.0"
        xsi:schemaLocation="urn:ietf:params:xml:ns:rgp-1.0 rgp-1.0.xsd">
        <rgp:rgpStatus s="redemptionPeriod" />
      </rgp:infData>
    </extension>
    <trID>
      <clTRID>ABC-12345</clTRID>
      <svTRID>54322-XYZ</svTRID>
    </trID>
  </response>
</epp>
```

Example **`<info>`** response extension for "pendingRestore" status (note that only the extension element changes from the first example): 
```xml
<extension>
  <rgp:infData xmlns:rgp="urn:ietf:params:xml:ns:rgp-1.0"
    xsi:schemaLocation="urn:ietf:params:xml:ns:rgp-1.0 rgp-1.0.xsd">
    <rgp:rgpStatus s="pendingRestore" />
  </rgp:infData>
</extension>
```

Example **`<info>`** response extension for "pendingDelete" status (note that only the extension element changes from the first example):
```xml
<extension>
  <rgp:infData xmlns:rgp="urn:ietf:params:xml:ns:rgp-1.0"
    xsi:schemaLocation="urn:ietf:params:xml:ns:rgp-1.0 rgp-1.0.xsd">
    <rgp:rgpStatus s="pendingDelete" />
  </rgp:infData>
</extension>
```

[↑ Back to Top](#table-of-contents)

---

### 2. Transform Commands

---

#### Update

Example **`<update>`** command without a restore report:
```xml
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
  <command>
    <update>
      <domain:update
        xmlns:domain="urn:ietf:params:xml:ns:domain-1.0"
        xsi:schemaLocation="urn:ietf:params:xml:ns:domain-1.0 domain-1.0.xsd">
        <domain:name>example.com</domain:name>
        <domain:chg />
      </domain:update>
    </update>
    <extension>
      <rgp:update xmlns:rgp="urn:ietf:params:xml:ns:rgp-1.0"
        xsi:schemaLocation="urn:ietf:params:xml:ns:rgp-1.0 rgp-1.0.xsd">
        <rgp:restore op="request" />
      </rgp:update>
    </extension>
    <clTRID>ABC-12345</clTRID>
  </command>
</epp>
```

Example **`<update>`** command with a restore report:
```xml
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
  <command>
    <update>
      <domain:update
        xmlns:domain="urn:ietf:params:xml:ns:domain-1.0"
        xsi:schemaLocation="urn:ietf:params:xml:ns:domain-1.0 domain-1.0.xsd">
        <domain:name>example.com</domain:name>
        <domain:chg />
      </domain:update>
    </update>
    <extension>
      <rgp:update xmlns:rgp="urn:ietf:params:xml:ns:rgp-1.0"
        xsi:schemaLocation="urn:ietf:params:xml:ns:rgp-1.0 rgp-1.0.xsd">
        <rgp:restore op="report">
          <rgp:report>
            <rgp:preData>Pre-delete registration data goes here.
              Both XML and free text are allowed.</rgp:preData>
            <rgp:postData>Post-restore registration data goes here.
              Both XML and free text are allowed.</rgp:postData>
            <rgp:delTime>2003-07-10T22:00:00.0Z</rgp:delTime>
            <rgp:resTime>2003-07-20T22:00:00.0Z</rgp:resTime>
            <rgp:resReason>Registrant error.</rgp:resReason>
            <rgp:statement>This registrar has not restored the
              Registered Name in order to assume the rights to use
              or sell the Registered Name for itself or for any
              third party.</rgp:statement>
            <rgp:statement>The information in this report is
              true to best of this registrar's knowledge, and this
              registrar acknowledges that intentionally supplying
              false information in this report shall constitute an
              incurable material breach of the
              Registry-Registrar Agreement.</rgp:statement>
            <rgp:other>Supporting information goes
              here.</rgp:other>
          </rgp:report>
        </rgp:restore>
      </rgp:update>
    </extension>
    <clTRID>ABC-12345</clTRID>
  </command>
</epp>
```

Example "restore request" **`<update>`** response:
```xml
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
  <response>
    <result code="1000">
      <msg lang="en">Command completed successfully</msg>
    </result>
    <extension>
      <rgp:upData xmlns:rgp="urn:ietf:params:xml:ns:rgp-1.0"
        xsi:schemaLocation="urn:ietf:params:xml:ns:rgp-1.0 rgp-1.0.xsd">
        <rgp:rgpStatus s="pendingRestore" />
      </rgp:upData>
    </extension>
    <trID>
      <clTRID>ABC-12345</clTRID>
      <svTRID>54321-XYZ</svTRID>
    </trID>
  </response>
</epp>
```

[↑ Back to Top](#table-of-contents)
