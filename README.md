This repo demonstrates a crash I have found in `v0.21.0-alpha.1` of `rustls`. When using it with `rustls-native-certs` to get the CAs trusted by the system on macOS Ventura, I found it fails to parse when reading one particular certificate. The same certificate is parsed by `v0.20.8` without errors.

## Running

To try the system keychain:

`cargo run system`

To use the embedded copy of the failing certificate:

`cargo run`

## Cert

The failing certificate is in the repo at `crash.pem`. The OpenSSL output for this is as follows:

```
$ openssl x509 -in crash.pem -noout -text
Certificate:
    Data:
        Version: 3 (0x2)
        Serial Number:
            e6:09:fe:7a:ea:00:68:8c:e0:24:b4:ed:20:1b:1f:ef:52:b4:44:d1
        Signature Algorithm: sha1WithRSAEncryption
        Issuer: C = PL, O = Krajowa Izba Rozliczeniowa S.A., CN = SZAFIR ROOT CA
        Validity
            Not Before: Dec  6 11:10:57 2011 GMT
            Not After : Dec  6 11:10:57 2031 GMT
        Subject: C = PL, O = Krajowa Izba Rozliczeniowa S.A., CN = SZAFIR ROOT CA
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
                Public-Key: (2048 bit)
                Modulus:
                    00:ac:47:2f:8f:59:31:39:a5:ea:0d:f0:a5:8c:2b:
                    bd:02:a4:bd:cd:0a:73:aa:09:e6:cc:5f:82:6a:75:
                    a9:97:e5:bb:06:ef:f3:c0:5d:a1:c1:d2:89:79:58:
                    f0:dc:eb:55:8c:ee:58:1a:7b:27:ff:4a:50:b3:00:
                    a1:6a:13:23:33:90:e8:2f:53:36:6e:18:6c:0f:40:
                    d6:37:57:d7:06:0d:2f:90:24:ca:57:fc:5b:92:ff:
                    23:80:57:85:3f:4a:9e:52:3a:c7:e6:bb:05:50:26:
                    9e:bf:d3:b6:5f:ae:b5:3f:c0:b2:6b:36:7d:17:c4:
                    b0:5d:af:64:f3:31:1f:85:05:a8:e0:21:16:3c:53:
                    92:62:56:7f:60:ae:e2:7b:d1:40:91:dc:b6:d0:7e:
                    70:40:28:6a:0f:e1:11:1b:7f:fa:dc:e8:e1:44:73:
                    eb:1e:ab:d7:db:68:62:6c:71:e0:58:e3:06:45:21:
                    59:0c:35:5f:6d:6b:7b:ff:c7:a1:fd:be:d1:88:22:
                    c1:84:fc:e3:32:db:7a:a0:ea:08:e3:0e:5f:1b:c7:
                    e3:cd:32:18:39:21:fd:0a:1a:cd:65:f7:53:be:47:
                    40:4c:53:fe:22:65:c2:7b:bb:23:55:7a:1d:7c:ed:
                    28:8c:02:06:3b:31:7d:f6:c7:de:33:ac:84:22:6d:
                    cb:07
                Exponent: 65537 (0x10001)
        X509v3 extensions:
            X509v3 Basic Constraints: critical
                CA:TRUE
            X509v3 Key Usage: critical
                Certificate Sign, CRL Sign
            X509v3 Subject Key Identifier:
                53:92:A3:7D:FF:82:76:F0:33:D4:EB:92:67:47:61:33:1B:68:3B:2A
    Signature Algorithm: sha1WithRSAEncryption
    Signature Value:
        39:50:55:9d:e4:42:ff:a4:1b:e2:20:c9:b5:cc:3d:89:2d:40:
        a9:a7:49:89:1b:b2:64:c3:39:c8:3b:36:b0:84:69:85:15:ac:
        46:a3:10:21:40:11:20:85:a3:fe:13:42:91:eb:aa:00:c1:50:
        ba:c5:ed:f6:41:45:52:1d:f5:aa:5a:77:73:6c:e0:f3:2c:1f:
        32:8f:b5:80:47:47:03:33:8e:59:9e:04:81:3c:1b:85:13:75:
        ae:18:b2:57:f6:0d:74:d0:44:df:21:7f:b8:60:14:7f:e0:d4:
        7f:ba:f4:e7:a6:77:7d:7a:d7:bb:5a:19:74:65:f6:3d:e3:2b:
        ae:e3:14:2a:07:94:05:bf:e3:fb:21:75:d5:95:33:a3:92:3b:
        86:5c:47:32:40:08:d0:cd:b1:5a:4f:db:c8:00:14:9b:78:25:
        d6:05:b8:ef:39:7b:a4:27:2e:96:80:08:9c:97:31:8d:49:8a:
        6b:ad:ef:11:cb:27:5d:7e:68:d6:f4:72:06:c2:c2:0e:d3:71:
        25:76:2b:ef:a8:46:0b:d4:7b:f5:c3:f3:ee:af:ee:85:50:43:
        9a:2f:af:0f:10:85:28:bd:f8:30:20:1d:81:03:0f:af:64:3b:
        b0:70:7d:26:5f:9e:3c:3e:88:c6:ca:0e:37:e1:c0:e3:91:25:
        da:de:61:c5
```
