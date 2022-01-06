---
layout: default
parent: Ncanode
title: CMS API
nav_order: 4
---

CMS API Reference
=============

`POST /cms/validate` - validate CMS signature by some rules.
Also returns in response signer details such as IIN, BIN, full name or email.

Parameters (json):
- cms [string,optional] - CMS-formatted signature
- iin [string,optional] - IIN
- bin [string,optional] - BIN
- data [string,optional] - Signature content to verify against CMS signed data.
- rule [array|string,optional] - rule(s) for validating. ['iin', 'bin', 'auth', 'individual', 'employee', 'ceo', 'organisation']
- constraints [array|optional] - array of sets {"iin", "bin", "rule"}, request will be successful, if any set is passed
- verify_ocsp [boolean|optional] - verify
- verify_crl [boolean|optional] - array of sets {"iin", "bin", "rule"}, request will be successful, if any set is passed
- checked_at [datetime|optional] - datetime to validate against, when certificate expiration is verified. Optional. Default is current datetime.

Example request:
```json
{
  "cms": "MIIg...",
  "iin": "111222333444",
  "bin": "111222333444",
  "rule": ["iin", "bin", "ceo"]
}
```

Request below is equivalent to above:
```json
{
  "cms": "MIIg...",
  "constraints": [
      {
          "iin": "111222333444",
          "bin": "111222333444",
          "rule": ["iin", "bin", "ceo"]
      }
  ]
}
```

Valid, if any "constraint" item passes:
```json
{
  "cms": "MIIg...",
  "constraints": [
      {
          "iin": "111222333444",
          "bin": "555666777888",
          "rule": ["iin", "bin", "ceo"]
      },
      {
          "iin": "999888777666",
          "bin": "555444333222",
          "rule": ["iin", "bin", "organisation"]
      }
  ]
}
```

Success response:
```json
{
  "status": true,
  "content": {
    "signer": {
      "iin": "999888777666",
      "bin": "555444333222",
      "org_name": "Some Organization",
      "first_name": "John",
      "last_name": "Doe",
      "mid_name": "Junior",
      "gender": "male",
      "email": "john@example.com",
      "birthday": "2000-12-12"
    }
  }
}
```

`GET /cms/extract` - extract signed data from CMS signature

Parameters (json):
- cms [string,optional] - CMS-formatted signature

Success response:
```json
{
  "status": true,
  "content": {
    "data": "signed data..."
  }
}
```

`POST /origin` - [Ncanode](https://ncanode.kz/) origin request.

Transparently proxies request to Ncanode API.
No need to set "p12" and "password" parameters.
Those parameters are set from environment variables `NCANODE_KEY` and `NCANODE_PWD`

Parameters (json):
- method [string,required] - method. Ex. 'XML.sign'
- version [string,optional] - ncanode api version. Default '1.0'
- params [array,optional] - array of params

Success response:
```json
{
  "status": true,
  "message": null,
  "content": {
    "origin": {
      "result": {
        "xml": "<?xml version=\"1.0\" encoding=\"utf-8\" standalone=\"no\"?><root><name>NCANode</name><ds:Signature xmlns:ds=\"http://www.w3.org/2000/09/xmldsig#\">\r\n<ds:SignedInfo>\r\n<ds:CanonicalizationMethod Algorithm=\"http://www.w3.org/TR/2001/REC-xml-c14n-20010315\"/>\r\n<ds:SignatureMethod Algorithm=\"http://www.w3.org/2001/04/xmldsig-more#rsa-sha256\"/>\r\n<ds:Reference URI=\"\">\r\n<ds:Transforms>\r\n<ds:Transform Algorithm=\"http://www.w3.org/2000/09/xmldsig#enveloped-signature\"/>\r\n<ds:Transform Algorithm=\"http://www.w3.org/TR/2001/REC-xml-c14n-20010315#WithComments\"/>\r\n</ds:Transforms>\r\n<ds:DigestMethod Algorithm=\"http://www.w3.org/2001/04/xmlenc#sha256\"/>\r\n<ds:DigestValue>ybvg7uzrmIoa6Q02yU8BiLjYNl64fr+yXCtg0kHwdv4=</ds:DigestValue>\r\n</ds:Reference>\r\n</ds:SignedInfo>\r\n<ds:SignatureValue>\r\niSO1UrZLWBsiMAybQEkgvz7VgGjfmixA==\r\n</ds:SignatureValue>\r\n<ds:KeyInfo>\r\n<ds:X509Data>\r\n<ds:X509Certificate>\r\nLCt2q\r\n</ds:X509Certificate>\r\n</ds:X509Data>\r\n</ds:KeyInfo>\r\n</ds:Signature></root>"
      }
    }
  }
}
```