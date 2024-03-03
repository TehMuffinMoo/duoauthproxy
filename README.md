# Duo Auth Proxy (LDAP/RADIUS) Docker Container

## Updates
There is a GitHub action setup to run every 12 hours, which will check for newer versions of the Duo Auth Proxy and update the docker package if found.

Each container release will be tagged with the version of Duo Authentication Proxy (i.e `duoauthproxy:6.3.0`), in addition to the `duoauthproxy:latest` tag being available for the newest release.

## Configuration
### Docker Compose Example
```
  duoauthproxy:
    image: ghcr.io/TehMuffinMoo/duoauthproxy:latest
    container_name: duoauthproxy
    restart: unless-stopped
    ports:
      - 55389:55389
    volumes:
      - ./duoauthproxy/authproxy.cfg:/opt/duoauthproxy/conf/authproxy.cfg
      - ./duoauthproxy/log/:/etc/duoauthproxy/log/
```

### Duo Config Example
`duoauthproxy/authproxy.cfg`
```assembly
; Complete documentation about the Duo Auth Proxy can be found here:
; https://duo.com/docs/authproxy_reference
; NOTE: After any changes are made to this file the Duo Authentication Proxy
; must be restarted for those changes to take effect.
; MAIN: Include this section to specify global configuration options.
; Reference: https://duo.com/docs/authproxy_reference#main-section
;[main]
; CLIENTS: Include one or more of the following configuration sections.
; To configure more than one client configuration of the same type, append a
; number to the section name (e.g. [ad_client2])
[ad_client]
host=10.1.1.10
service_account_username=ldapuser
service_account_password=somesupersecurepassword
search_dn=DC=Your,DC=domain
; SERVERS: Include one or more of the following configuration sections.
; To configure more than one server configuration of the same type, append a
; number to the section name (e.g. radius_server_auto1, radius_server_auto2)
[ldap_server_auto]
ikey=DuoIntegrationKey
skey=DuoSecretKey
api_host=DuoAPIFQDN
factors=auto
failmode=safe
client=ad_client
port=55389
```
