# gravitee-secret-provider-cyberark-conjur

## Category

`secret-provider`

## Compatibility Matrix

| Gravitee Version | Secret Provider CyberArk-Conjur | Product |
|------------------|---------------------------------|---------|
| 4.8.x            | 1.0.x                           | All     |


## Overview

Get secret from CyberArk Conjur Secret Manager.

Authentication can be made via login & apikey credentials (and optional certificate) authentication mechanism.

## Configuration

These are example configurations in `gravitee.yml`.

### Configuration-level Secrets

Will allow `secrets://cyberark-conjur/...` in gravitee.yml

```YAML
secrets:
  cyberark-conjur:
    enabled: true
    applianceurl: ... 
    account: ...
    auth:
      provider: apikey
      config:
        login: ...
        apikey: ...
        # Other optional settings
        # sslVerify: true
        # sslCertificateFile: ....crt
```

### API-level Secrets
```YAML
api:
  secrets:
    providers:
      - plugin: cyberark-conjur
        configuration:
          enabled: true
          applianceurl: ... 
          account: ...
          auth:
            provider: apikey
            config:
              login: ...
              apikey: ...
              # Other optional settings
              # sslVerify: true
              # sslCertificateFile: ....crt
```

And then to retrieve a secret from CyberArk Conjur, use the following syntax in an EL-supported field:
```
{#secrets.get('/cyberark-conjur/{myApplicationName}/{mySecretKeyName}:secretValue')}
```
Example:
```
{#secrets.get('/cyberark-conjur/BotApp/secretVar:secretValue')}
```

### Docker Compose Example
```YAML
services:
  ...
  gateway:
    ...
    environment:
      # SECRET MANAGER - CyberArk Conjur
      - gravitee_api_secrets_providers_0_plugin=cyberark-conjur
      - gravitee_api_secrets_providers_0_configuration_enabled=true
      - gravitee_api_secrets_providers_0_configuration_applianceurl=https://conjur-host
      - gravitee_api_secrets_providers_0_configuration_account=...
      - gravitee_api_secrets_providers_0_configuration_auth_provider=apikey
      - gravitee_api_secrets_providers_0_configuration_auth_config_login=...
      - gravitee_api_secrets_providers_0_configuration_auth_config_apikey=...
      # - gravitee_api_secrets_providers_0_configuration_auth_config_sslVerify=true #default=true (Should never be disabled used in production)
      # - gravitee_api_secrets_providers_0_configuration_auth_config_sslCertificateFile=your_cert.crt #.crt file (trusted)
```
      
## Known Limitations

* JSON Secrets only (no plain text, or binary)
* No watch, although secret may contain X.590 pair, but won't be renewed upon update.

