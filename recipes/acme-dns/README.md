The acme-dns HTTPS API is reverse-proxied through `https://acme-dns.<FQDN>:8443` .

> ℹ️ **Info:** Unlike default acme-dns setup, the API is **not accessible** at `https://auth.<your-domain>/` !

----

To start, port forward UDP & TCP [your-public-ip]:53 to [k8s-machine]:30053 (DNS standards mandate the use of Port 53 but Kubernetes does not allow :53 as a NodePort by default).

Then create the following records at your DNS provider:

```
auth.your-domain.test. NS auth-ns.your-domain.test.
auth-ns.your-domain.test. A 0.0.0.0 # Replace 0.0.0.0 with your public IP.
```

If you're using a dynamic DNS service where `mybox.ddns-provider.test` points to your public address, you just need one `NS` record to point at it.

Don't forget to update the `nsname` variable in the recipe to match.

```
auth.your-domain.test. NS mybox.ddns-provider.test.
```

You can then follow the official testing instructions (just remember to use the base URL mentioned above instead of `auth.example.org`):

https://github.com/joohoi/acme-dns?tab=readme-ov-file#testing-it-out

You may have to ignore certificate warnings if `<FQDN>` is not a public domain or if the default certificate is selfsigned, e.g.
`curl --insecure -X POST https://acme-dns.<FQDN>:8443/register`
