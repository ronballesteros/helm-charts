# **Bitgo-Express**
A customized helm chart for deploying bitgo-express application in a kubernetes cluster.

More information/installation requirements about Bitgo can be found [here](https://developers.bitgo.com/guides/get-started/express/install).

Per the Bitgo documentation (and best practices), it is recommended to enable HTTPS when running Bitgo in production. However, since all communication will be from within the cluster, just like how other microservices communicate, this can be done over http. Though, we can still enable SSL as the helm chart supports it but I think it would be better to enable service to service communication via SSL with a service mesh in the cluster.

## Enabling HTTPS

To enable HTTPS, this will require certificates.  More information can be found [here](https://developers.bitgo.com/guides/get-started/express/production).  To enable HTTPS, this can be done via Vault (required for production/live) or by adding the cert values in the values.yaml file (testing/dev).

The configuration:
- sets ssl.enabled to true.
- will create a kubernetes secret resource called "bitgo-ssl-cert". that contains the cert and key.
- place the cert in location `/etc/ssl/certs`
  

```
ssl:
  enabled: true
  secretName: "bitgo-ssl-cert"
  key: |
    -----BEGIN PRIVATE KEY-----
    MIIEvwIBADANBgkqhkiG9w0BAQEFAASCBKkwggSlAgEAAoIBAQDMxnXsXvZG3tw
    ......
    -----END PRIVATE KEY-----
  cert: |
    -----BEGIN CERTIFICATE-----
    MIIDLjCCAhYCCQCyriS6QAGr7zANBgkqhkiG9w0BAQsFADBZMQswCQYDVQQGEwJV
    ...
    -----END CERTIFICATE-----
  path: /etc/ssl/certs
  ```

## Container Environment Variables
Environment variables can be added to the bitgo container by adding the key/value pair under the `env` spec as `key: value` (example: APP: BITGO). 
The following settings will be added as environment variables.
```
env:
  NODE_ENV: production
  BITGO_ENV: prod
  BITGO_PORT: 3080
  BITGO_KEYPATH: /etc/ssl/certs/tls.key
  BITGO_CRTPATH: /etc/ssl/certs/tls.crt
```