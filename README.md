# deckwatch-plugin-memcached

A [deckwatch](https://github.com/alexconrey/deckwatch) plugin that provisions a Memcached
sidecar for an application via the **Infrastructure tab** — no annotations required.

## Usage

1. Open the Application detail page in deckwatch.
2. Click the **Infrastructure** tab.
3. Click **+ Add Memcached**.
4. Fill in any optional fields (image) and click **Provision**.

Once provisioned, deckwatch injects on the next reconcile:

- **Sidecar**: `memcached:1.6-alpine` on port 11211 (50m CPU / 64Mi memory), named `memcached-sidecar`
- **Env var**: `MEMCACHED_URL=memcached://localhost:11211` into every deployment in the application
- **Annotation**: `deckwatch.io/memcached: "provisioned"` on all deployments

To remove the sidecar, click **Deprovision** on the Infrastructure tab. The sidecar is removed
from all deployments on the next reconcile via server-side apply.

## Configuration

### Resource fields (per-provisioned instance)

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| `MEMCACHED_IMAGE` | String | `memcached:1.6-alpine` | Memcached container image |

### Plugin config (global defaults)

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| `MEMCACHED_IMAGE` | String | `memcached:1.6-alpine` | Default Memcached image (overridable per-resource) |

Set global defaults in **Settings → Plugins → Memcached → Configure**.

## Building

```bash
# Install the wasm32 target (once)
rustup target add wasm32-unknown-unknown

# Build the WASM artifact
make build          # → dist/plugin.wasm

# Run host-target unit tests
make test
```

## Marketplace

This plugin is available on the [deckwatch marketplace](http://market.deckwatch.io).

To install via the deckwatch UI: **Settings → Plugins → Browse Marketplace → Memcached**.

### Submitting to the marketplace

To add or update this plugin in the marketplace catalog, open a PR against
[alexconrey/deckwatch-marketplace](https://github.com/alexconrey/deckwatch-marketplace)
adding the following entry to `catalog.json`:

```json
{
  "name": "Memcached",
  "slug": "deckwatch-plugin-memcached",
  "description": "Provisions a Memcached sidecar via the Application Infrastructure tab.",
  "author": "alexconrey",
  "homepage": "https://github.com/alexconrey/deckwatch-plugin-memcached",
  "trust_level": "community",
  "tags": ["memcached", "cache", "sidecar"],
  "latest_version": "0.1.0",
  "source": {
    "type": "github",
    "repo": "alexconrey/deckwatch-plugin-memcached",
    "ref": "v0.1.0",
    "path": "plugin.wasm",
    "use_release": true
  },
  "allowed_hosts_hint": []
}
```

Also add `plugins/deckwatch-plugin-memcached.md` following the template in the marketplace repo.

## License

Apache-2.0
