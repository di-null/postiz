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

## Channels
### TikTok
#### Postiz Configuration

- Add there properties to Postiz configuration file under `postiz/environment`
- Create corresponding environmental variables and assign `Client Key` and `Client Secret` values to them. `Client Key` and `Client Secret` you should be taken from your application configuration inside TikTok Developers account.

```
TIKTOK_CLIENT_ID: "${TIKTOK_CLIENT_ID}"
TIKTOK_CLIENT_SECRET: "${TIKTOK_CLIENT_SECRET}"
```

#### Application settings
##### Description
```
This is for personal use with self-hosted instance of Postiz for scheduling my own social media content.
```
##### Terms of Service and Privacy Policy
Inside `/uploads` folder execute next commands:

```
cat > terms.html << 'EOF'
<!DOCTYPE html>
<html>
<head><title>Terms of Service</title></head>
<body>
<h1>Postiz Terms of Service</h1>
<p>This is a personal self-hosted instance of Postiz running on my private Dokploy server.</p>
<p>It is used only by me to schedule and post content to my own social media accounts.</p>
<p>Last updated: May 2026</p>
</body>
</html>
EOF
```

```
cat > privacy.html << 'EOF'
<!DOCTYPE html>
<html>
<head><title>Privacy Policy</title></head>
<body>
<h1>Postiz Privacy Policy</h1>
<p>This self-hosted Postiz tool only stores data on my own server.</p>
<p>No data is shared with third parties except when I explicitly ask it to post to TikTok.</p>
<p>Last updated: May 2026</p>
</body>
</html>
EOF
```
