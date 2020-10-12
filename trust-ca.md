### Trust CA

Importing and trusting a CA itself is not a safe as allows to issue any certificates with CA,
which is why we want to limit CA to some sub-domain, to make it harmless.


### Checking if CA has constraints
Make sure that the CA has [X509v3 Name Constraints](https://www.openssl.org/docs/man1.0.2/man5/x509v3_config.html)

```
X509v3 Name Constraints: critical
    Permitted:
      DNS:.mydomain.com
```

### Test with curl

```bash
curl --cafile ca.crl http://mydomain.com
```

### Add root CA on Linux
This will install root CA to the system as trusted

```bash
cp ca.crl /usr/local/share/ca-certificates/my-root-ca.crl
update-ca-certificates
```

### Firefox

[Add CA to Firefox](https://support.mozilla.org/en-US/kb/setting-certificate-authorities-firefox)

### Mac os
