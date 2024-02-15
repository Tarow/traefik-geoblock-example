# Traefik with Geoblock Example

This is an example project showing how Traefik can be used with various middlewares defined in a [file provider](https://doc.traefik.io/traefik/providers/file/). Services are detected using Docker labels.

The example setup includes the following:

- Automatic Let's Encrypt wildcard certificate generation
- Middleware for internal only access (private IP ranges)
- Middleware for publicy exposed services (includes geoblock, ratelimit & security-headers)

For geoblocking the [nscuro geoblock plugin](https://github.com/nscuro/traefik-plugin-geoblock) is being used.

## Setup

1. Replace all occurences of `mydomain.com` with your own domain.
2. In order to get certificates, set the necessary environment variable in the [compose.yml](traefik/compose.yml). This example uses Cloudflare as a provider, you can find the necessary environment variables for your provider [here](https://doc.traefik.io/traefik/https/acme/#providers).
3. Create the docker network used by Traefik: `docker network create traefik-proxy`
4. Run the container: `docker compose -f traefik/compose.yml up -d`

## Middlewares

For testing/demonstration purposes, the repo also contains two [whoami](whoami/compose.yml) services.
One of them uses the private middleware chain, the other one the public middleware.

The private whoami service can only be accessed from internal IP addresses. The service using the public chain can be accessed from outside internal IP ranges. In order to increase security when exposing public services, it applies security-headers, ratelimits and geoblocking. In this example configuration, only requests from Germany are allowed.

In order to further improve security for exposed services, consider adding something like [CrowdSec](https://www.crowdsec.net/).
