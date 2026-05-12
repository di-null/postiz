# `Postiz` installation guide on `Dokploy`

## Prerequisites
- `Postiz` template in `Dokploy` is outdated therefore provided configuration by default is not full since it missing `Temporal` configuration.
- I recommend you not to add services via `Template`. Use `Compose` instead and add `Raw` configuration manually.
- You can use my `docker-compose.yaml`. This version here is the cleanest I was able to make so far.

## Environmental Variables
- POSTIZ_HOST=`your custom domain or localhost`
- POSTIZ_DB_NAME=`you can use any (e.g. postiz)`
- POSTIZ_DB_USER=`you can use any (e.g. postiz)`
- POSTIZ_DB_PASSWORD=`your preferable password (I used password manager to generate me one)`
- TEMPORAL_DB_USER=`you can use any (e.g. temporal)`
- TEMPORAL_DB_PASSWORD=`your preferable password (I used password manager to generate me one)`
- JWT_SECRET=`generate one using terminal command: openssl rand -hex 32`

## Custom domain/subdomain setup
I recommend using subdomain so you can use the same domain name for different services running on your server.
### Postiz service
In the domains tab make sure you have next:
- Service Name: `postiz container name (e.g. postiz, postiz-app)`
- Host: `your domain/subdomain (e.g. postiz.your.domain)`
- Container Port: `5000`
- HTTPS: `On`
- Certificate Provider: `Let's Encrypt`
### Cloudflare
Assuming you already have your tunnel running. I created mine as additional service in `Dokploy`. Same recommendation is here not to create it using `Template` but `Compose` instead. I ran into some strange caching issue when created from `Template` was always failing for me. Just use the same configuration but create it as `Compose` instead. Do not forget to add `Token` from cloudflare tunnel to `Environmental Variables`
Go to your tunnel in Zero Trust -> Published application routes -> Add new route with following parameters:  
- Subdomain: `your preferred subdomain (e.g. postiz)`
- Domain: `select your domain`
- Type: `HTTPS`
- URL: `127.0.0.1:443` (This is url to `Traefik` that is running on `Dokploy` by default that will reverse proxy request to your `Postiz`)
- Origin request and connection settings -> TLS -> No TLS Verify: `On` (This will allow to accept certificate from `Traefik`)

### Wish you good luck :)
