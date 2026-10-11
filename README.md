# Cache Domains

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![GitHub last commit](https://img.shields.io/github/last-commit/CoportonWS/cache-domains)](https://github.com/CoportonWS/cache-domains/commits/main)

A curated collection of CDN hostnames for game launchers, console networks,
operating-system updates, and software distribution platforms. The lists can be
used by LAN caches and DNS-based routing systems to reduce repeated downloads,
save upstream bandwidth, and improve delivery speeds on multi-device networks.

## Highlights

- Plain-text domain lists that are easy to audit and integrate
- A machine-readable service index in [`cache_domains.json`](cache_domains.json)
- Configuration generators for Dnsmasq, Unbound, AdGuard Home, Squid, and BIND RPZ
- Support for dedicated cache IPs, shared cache IPs, and multiple cache nodes
- Coverage for popular platforms including Steam, Epic Games, Blizzard, Riot,
  PlayStation, Xbox, Nintendo, Windows Update, and more

## Use cases

This repository is useful for:

- LAN parties and esports events
- Gaming cafes, schools, offices, and shared networks
- Home labs with multiple gaming PCs or consoles
- Network administrators operating a LAN cache or caching proxy

## Repository structure

| Path | Purpose |
| --- | --- |
| `*.txt` | One hostname or wildcard domain per line, grouped by service |
| [`cache_domains.json`](cache_domains.json) | Service metadata and the files assigned to each service |
| [`scripts/`](scripts/) | Configuration generators and an example mapping file |
| [`faq.md`](faq.md) | Common questions and operational guidance |

Lines beginning with `#` are comments. Entries beginning with `*.` represent
wildcard subdomains.

## Quick start

### Use the lists directly

Choose the service files required by your environment and feed their entries
into your DNS-routing or caching configuration. For integrations that need to
discover services automatically, use `cache_domains.json` as the index.

For example, [`steam.txt`](steam.txt) contains the domains associated with
Steam content delivery, while [`windowsupdates.txt`](windowsupdates.txt)
contains domains used for Microsoft and Windows updates.

### Generate DNS or proxy configuration

The included scripts require Bash and [`jq`](https://jqlang.github.io/jq/).

```bash
git clone https://github.com/CoportonWS/cache-domains.git
cd cache-domains/scripts
cp config.example.json config.json
```

Edit `config.json` to map each cache group to its IP address and enable the
services you want to route. A minimal shared-cache configuration looks like:

```json
{
  "ips": {
    "generic": ["10.10.10.200"]
  },
  "cache_domains": {
    "blizzard": "generic",
    "epicgames": "generic",
    "steam": "generic",
    "wsus": "generic"
  }
}
```

Run the generator that matches your DNS or proxy software:

```bash
bash create-dnsmasq.sh
bash create-unbound.sh
bash create-adguardhome.sh
bash create-squid.sh
bash create-rpz.sh
```

Generated files are written beneath `scripts/output/`. Review them before
installing them into a production DNS or proxy configuration. See the
[`scripts` documentation](scripts/README.md) for implementation-specific steps.

## Supported service groups

The current index includes:

- Game platforms: Steam, Epic Games, Blizzard/Battle.net, EA/Origin, Ubisoft,
  Riot Games, Rockstar Games, and Wargaming
- Consoles and devices: PlayStation, Xbox Live, Nintendo, and iOS updates
- Games and publishers: Call of Duty, PUBG Mobile, Warframe, Path of Exile,
  The Elder Scrolls Online, Daybreak, ArenaNet, and others
- Software delivery: Windows Update and other common software CDNs

Refer to [`cache_domains.json`](cache_domains.json) for the authoritative and
current list.

## Contributing

Contributions that add, remove, or correct cacheable CDN domains are welcome.
When proposing a change:

1. Add the domain to the appropriate service file, one entry per line.
2. Use `#` for comments and `*.` only when the entire subdomain range is valid.
3. Update `cache_domains.json` when adding a new service or domain file.
4. Explain how the domains were identified and, when possible, how they were
   verified as cacheable content endpoints.
5. Avoid authentication, account, telemetry, API, and manifest endpoints unless
   they are demonstrably safe and necessary for content caching.

## Important notes

- CDN infrastructure changes over time. Validate entries against your own DNS
  logs and application behavior before relying on them in production.
- A domain appearing in this repository does not guarantee that all traffic on
  it is cacheable. Configure your cache according to the relevant provider's
  behavior and your local policies.
- HTTPS interception, where applicable, can introduce security and privacy
  risks. This repository provides domain data and configuration generators; it
  does not recommend bypassing certificate validation or platform security.

More answers are available in the [FAQ](faq.md).

## License

Distributed under the [MIT License](LICENSE).

