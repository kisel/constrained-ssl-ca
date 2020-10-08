### mkcert script

generates CA restricted to domain and https certificates signed with this CA
This removes the risk of importing and trusting unrestricted CA


### Demo

```bash
export CA_PASS=mysecret
mkdir -p example.com
cd example.com
../mkcert genca example.com '/C=US/O=Test CA for example.com/OU=CA'
../mkcert gencert demo.example.com demo.example.com
../mkcert gencert api.example.com api.example.com
../mkcert gencert '*.example.com' wildcard.example.com

demo.example.com
```


### Generating restricted CA
Following
```bash
mkdir -p mydomain
cd mydomain
export CA_PASS=<mysecretpassword for CA>
../mkcert genca .mydomain.com
Generating RSA private key, 4096 bit long modulus (2 primes)
...
Getting Private key
```

### Checking ca restriction

```bash
cd mydomain
../mkcert showca
        X509v3 extensions:
            X509v3 Basic Constraints: critical
                CA:TRUE
            X509v3 Key Usage: critical
                Certificate Sign, CRL Sign
            X509v3 Subject Key Identifier: 
                ...
            X509v3 Name Constraints: critical
                Permitted:
                  DNS:.mydomain.com
```

### Create a certificate
```
cd mydomain
./mkcert mkcert mydomain.com
```


### Test constraint
Testing certificate for host signed with CA root.
Sertificate signature will not be accepted

```
Firefox shows "Error code: SEC_ERROR_CERT_NOT_IN_NAME_SPACE"
```

```
$ openssl verify -CAfile ca.crt test_unrelated_domain.crt
error 47 at 0 depth lookup: permitted subtree violation
error testserver.crt: verification failed
```

```
$ curl https://unrelated-domain.com/
* SSL certificate problem: permitted subtree violation
```

