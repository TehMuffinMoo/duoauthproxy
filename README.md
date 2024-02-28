# Duo Auth Proxy (LDAP/RADIUS) Docker Container

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
```
