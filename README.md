# Cache Autopilot

Cache Autopilot is a WordPress cache freshness plugin that keeps cached pages fresh after content changes — automatically. Instead of flushing everything on every save, it determines exactly which pages are affected, purges only those entries through your active cache plugin, and rebuilds them through controlled warmup execution.

This repository contains the public changelog for Cache Autopilot:

* see [Cache Autopilot changelog](https://ekesto.github.io/cache-autopilot-site/docs/changelog/cache-autopilot/)

Documentation lives on the product website. This repository exists only for release transparency and changelog history.

Visit the website: [wpcacheautopilot.com](https://wpcacheautopilot.com)

## How it works

Cache Autopilot does not replace cache plugins. Cache plugins handle storage and delivery. Cache Autopilot handles cache freshness — the part most cache plugins leave to a full flush.

Lifecycle handled:

1. Detect content changes
2. Resolve affected URLs
3. Purge via cache adapter
4. Queue warmup requests
5. Rebuild cache via paced execution

## Included engines

### Cache Invalidator

Automatically purges the right WordPress cache entries after content changes — without clearing the entire cache.

Core capabilities:

* Post type–based invalidation rules
* Related post and cross-content invalidation
* Archive URL inclusion
* Multilingual support
* Timed invalidation rules for time-sensitive content
* Scheduled post support — cache refreshes at publish time
* Global invalidation controls for theme and template changes
* Developer hooks and filters for custom integrations

### Cache Warmup

Rebuilds cached pages after invalidation so visitors don't hit cold pages.

Core capabilities:

* Targeted cache warming for purged URLs
* Full site cache preloading from sitemaps
* Auto-paced queue execution via WP-Cron
* Priority ordering for high-value pages
* Diagnostics and run history

## Integrations

Cache Autopilot integrates with widely used WordPress cache plugins. Behavior depends on adapter capabilities and hosting environment.

See [Supported Integrations](https://wpcacheautopilot.com/docs/supported-integrations/) for the full compatibility list.

## Who it's for

Cache Autopilot is built for WordPress developers, agencies managing multiple client sites, and technical site owners who need reliable, predictable cache behavior in production — without manual cache management.

## Links

Website:
[wpcacheautopilot.com](https://wpcacheautopilot.com)

Documentation:
[wpcacheautopilot.com/docs](https://wpcacheautopilot.com/docs/)

Getting Started:
[wpcacheautopilot.com/docs/getting-started](https://wpcacheautopilot.com/docs/getting-started/)

Support:
[wpcacheautopilot.com/support](https://wpcacheautopilot.com/support/)

## Maintainer

Developed and maintained by Beat Schenkel (ekesto), an independent WordPress developer:
[https://ekesto.com](ekesto.com)
