## OVHcloud DNS v6 API provider for octoDNS

An [octoDNS](https://github.com/octodns/octodns/) provider that targets [OVHcloud DNS](https://www.ovhcloud.com/en/domains/dns-subdomain/).

### Installation

#### Command line

```
pip install octodns-ovh
```

#### requirements.txt/setup.py

Pinning specific versions or SHAs is recommended to avoid unplanned upgrades.

##### Versions

```
# Start with the latest versions and don't just copy what's here
octodns==0.9.14
octodns-ovh==0.0.1
```

##### SHAs

```
# Start with the latest/specific versions and don't just copy what's here
-e git+https://git@github.com/octodns/octodns.git@9da19749e28f68407a1c246dfdf65663cdc1c422#egg=octodns
-e git+https://git@github.com/octodns/octodns-ovh.git@ec9661f8b335241ae4746eea467a8509205e6a30#egg=octodns_ovh
```

### Configuration

The provider supports two authentication methods. They are mutually exclusive: configure one or the other.

#### Application key

```yaml
providers:
  ovh:
    class: octodns_ovh.OvhProvider
    # OVH api v6 endpoint
    endpoint: ovh-eu
    # API application key
    application_key: env/OVH_APPLICATION_KEY
    # API application secret
    application_secret: env/OVH_APPLICATION_SECRET
    # API consumer key
    consumer_key: env/OVH_CONSUMER_KEY
```

#### OAuth2 service account

Uses the OAuth2 client credentials of an [OVHcloud IAM service account](https://docs.ovhcloud.com/en/guides/manage-and-operate/api/manage-service-account), whose rights are granted through IAM policies. Available on the `ovh-eu`, `ovh-ca` and `ovh-us` endpoints.

```yaml
providers:
  ovh:
    class: octodns_ovh.OvhProvider
    # OVH api v6 endpoint
    endpoint: ovh-eu
    # OAuth2 client id of the service account
    client_id: env/OVH_CLIENT_ID
    # OAuth2 client secret of the service account
    client_secret: env/OVH_CLIENT_SECRET
```

#### Values left out

A value left out of the provider configuration falls back to [python-ovh's own lookup](https://github.com/ovh/python-ovh#configuration): the `OVH_APPLICATION_KEY`, `OVH_APPLICATION_SECRET`, `OVH_CONSUMER_KEY`, `OVH_CLIENT_ID` and `OVH_CLIENT_SECRET` environment variables, then its `ovh.conf` files. Values of the other method found there count as configured: an OAuth2 configuration fails with `Can't use both application_key/application_secret and OAuth2 client_id/client_secret` while `OVH_APPLICATION_KEY` is still exported.

### Support Information

#### Records

OvhProvider supports A, AAAA, CAA, CNAME, DKIM, MX, NAPTR, NS, PTR, SPF, SRV, SSHFP, and TXT

#### Dynamic

OvhProvider does not support dynamic records.

### Development

See the [/script/](/script/) directory for some tools to help with the development process. They generally follow the [Script to rule them all](https://github.com/github/scripts-to-rule-them-all) pattern. Most useful is `./script/bootstrap` which will create a venv and install both the runtime and development related requirements. It will also hook up a pre-commit hook that covers most of what's run by CI.
