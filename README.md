# Accounts

## Core Idea: Identity-Based Multi-Tenancy

- Log in to services with a single master identity and manage all business containers (weibeld, W5D Labs, w5d.io) from there
- These container entities in services are often called "organisations", "accounts", or "tenants"
  - For example, GitHub organisations, Azure tenants
- Can only be applied to services that provide such entities
  - For example, GitHub, AWS, Google Cloud, Azure, Cloudflare, HCP, HCP Terraform

## Alternative

- Separate master identity for each business container
  - For example, separate logins into a service for weibeld with a `*@weibeld.dev` email address, for W5D Labs with a `*@w5dlabs.com` email address, etc.
- Cons: login management overvhead, fragmentation
- Should only be used for services that don't provide multi-tenancy

## Correspondence to Hub-and-Spoke Model

- The w5d.io container acts as the "hub": providing services that at least two of the other containers depend on
- The other containers are the "spokes": plugging in to the services provided by the hub
- Some accounts and services are purely in the domain of the hub
  - For example, domain names, DNS settings, email (if we require that these things may only be managed by w5d.io)
  - Require only a single organisation entity for the hub container w5d.io (e.g. Porkbun, Namecheap)
- Other accounts and services may be accessed by spokes directly
  - For example, cloud platforms (AWS, etc.), HCP Terraform (might need to be decided for each service)
  - Separate organisation entities for all the spoke containers that access it (weibeld, W5D Labs, etc.)

## Open Questions

### Master identity

What email address should be used as the "master key" to the services?

- `danielmweibel@gmail.com`
  - Stable and persistent, externally managed, unlikely to be lost or changed, but tied to personal identity
- `*@w5d.io`
  - Not tied to personal identity, neutral, but self-managed, potentially unstable
  - Tied to platform container (w5d.io): are all accounts and services in the domain of the platform? May other containers have their own "private" services?

### Identity email constraint

Does a principle like "never use a self-managed email address to log in to a service" make sense?

- If using an email managed on own infrastructure to log in to the same infrastructure: risk of lockout when email stops working (implementation failure, domain expiry, etc)
- Instead use externally managed email addresses (e.g. Gmail)
- Always use a key from at least one level "upstream" to log in to the infrastructure at any given level
  - Analogy: don't keep the spare key to a house inside that house
