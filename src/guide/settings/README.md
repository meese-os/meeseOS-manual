---
description: A guide on customizing settings adapters
full_title: Settings Guide
---

# Settings Guide

MeeseOS provides options for customizing the settings storage.

Two adapters are provided by default:

* `localStorage` (in-browser, default)
* `server` (via server)

## Configuring adapter

See [official resource list](/resource/official/README.md) for provided adapter.

> The README file of the module should provide more specific examples.

> See [provider guide](../provider/README.md) for more information about provider setup.

### Client

```javascript
meeseOS.register(SettingsServiceProvider, {
  args: {
    adapter: "server"
  }
});
```

### Server

```javascript
const customAdapter = require("custom-adapter");

meeseOS.register(SettingsServiceProvider, {
  args: {
    adapter: customAdapter
  }
});
```

### Storing on filesystem

You can use the provided `fs` adapter to store settings on a filesystem:

> Settings are stored in `home:/.meeseOS/settings.json` by default.

```javascript
// client
meeseOS.register(SettingsServiceProvider, {
  args: {
    adapter: "server"
  }
});

// server
meeseOS.register(SettingsServiceProvider, {
  args: {
    adapter: "fs",
  }
});
```

## Configure adapter settings

The `config` parameter is passed on from your service provider registration:

```javascript
meeseOS.register(SettingsServiceProvider, {
  args: {
    adapter: customAdapter
    config: { /* Your configuration here */}
  }
});
```

If you have sensitive information in your configuration, consider using [complex-dotenv-json](https://github.com/meeseOS/complex-dotenv-json).
