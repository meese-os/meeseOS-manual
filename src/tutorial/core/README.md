---
description: This tutorial shows you how to interact with the core.
full_title: Core Tutorial
---

# Core Tutorial

This tutorial shows you how to interact with the core.

## Usage

The `core` variable injected into API signatures contains a reference to the `Core` instance.

It is used to interact with service providers, read configuration(s) and other core functionality.

### Common

These methods are shared between the server and client:

```javascript
// Gets a configuration value
const value = core.config("resolve.by.key", "optional default value");

// Retrieves an instance of a service
const service = core.make("namespace/service");

// Registers a new ServiceProvicer class
core.register(SomeServiceProvider, {/* registration options */});

// Registers a new singleton factory for service
core.singleton("namespace/service", () => new SomeService());

// Registers a new instance factory for service
core.instance("namespace/service", () => new SomeService());

// Checks if given service exists
const exists = core.has("namespace/service");

// Subscribe to an event
core.on("event-name", () => {});
```

### Client

The client has some extra methods for dealing with user data, requests, resources and applications:

```javascript
// Creates a URL based on the public path
const url = core.url("/foo/bar");

// Creates a new fetch() request
const promise = core.request("http://url", {/* options */}, "type")

// Launches an application
core.run("Preview", {file: {path: "home://image.png"}})

// Launches a new application based on a file
core.open({path: "home://image.png", mime: "image/png"});

// Gets user data
const user = core.getUser();

// Send an event to the server
core.send("event-name", 1, 2, 3);
```

#### Global "meeseOS" namespace

The window global `meeseOS` also lets you reach some of the core functionality.

```javascript
// Same as above (but some services are restricted)
meeseOS.make();

// These are the same as above
meeseOS.open();
meeseOS.request();
meeseOS.run();
meeseOS.url();
```

#### Client Events

* `init => ()` - Main init
* `meeseOS/core:boot => ()` - Core boot (before providers have initialized)
* `meeseOS/core:booted => ()` - Core booted (after providers have initialized)
* `meeseOS/core:start => ()` - Core start (before providers have started)
* `meeseOS/core:started => ()` - Core started (after providers have started)
* `meeseOS/core:logged-in => ()` - User successfully logged in
* `meeseOS/core:connect => ()` - Connection established
* `meeseOS/core:disconnect => ()` - Connection lost
* `meeseOS/core:destroy => ()` - Core destroy
* `meeseOS/application:launch => (name, args, options)` - Application pre-launch
* `meeseOS/application:launched => (name, app)` - Application launched
* `meeseOS/application:*:launched => (app)` - Application launched (`*` is metadata name)
* `meeseOS/application:create => (app)` - Application create
* `meeseOS/application:destroy => (app)` - Application destroy
* `meeseOS/window:create => (win)` - Window create
* `meeseOS/window:render => (win)` - Window render
* `meeseOS/window:change => (win, key, value)` - Window changed
* `meeseOS/window:transitionend => (ev, win)` - Window transition ended
* `meeseOS/desktop:transform => (rect)` - Desktop transformed
* `meeseOS/fs:mount => ()` - Filesystem mounted
* `meeseOS/fs:unmount => ()` - Filesystem unmounted
* `meeseOS/settings:save => ()` - Settings saved
* `meeseOS/settings:load => ()` - Settings loaded
* `meeseOS/vfs:* => (...args)` - VFS Method call started (`*` is method name)
* `meeseOS/vfs:*:done => (...args)` - VFS Method call finished (`*` is method name)
* `meeseOS/tray:create => (entry)` - Tray entry created
* `meeseOS/tray:remove => (entry)` - Tray entry removed
* `meeseOS/tray:update => (entries)` - Tray entry updated
* `meeseOS/notification:create => (notif)` - Notification created
* `meeseOS/notification:destroy => (notif)` - Notification destroyed

#### Client Services

These are the default provided services and their signatures:

* `meeseOS/application => (data)` - Creates a new [Application](../application/README.md) [instance](https://meeseOS.github.io/meeseOS-manual/api/meeseOS-client/class/src/application.js~Application.html)
* `meeseOS/window => (options)` - Creates a new [Window](../window/README.md) [instance](https://meeseOS.github.io/meeseOS-manual/api/meeseOS-client/class/src/window.js~Window.html)
* `meeseOS/event-handler => (name)` - Creates a new [EventEmitter](../bus/README.md) [instance](https://meeseOS.github.io/meeseOS-manual/api/meeseOS-common/class/src/event-handler.js~EventHandler.html)
* `meeseOS/websocket => (...args)` - Creates a new [WebSocket](../application/README.md#websockets) [instance](https://meeseOS.github.io/meeseOS-manual/api/meeseOS-client/class/src/websocket.js~Websocket.html)
* `meeseOS/notification => (options?)` - Creates a new [Notification](../notification/README.md) entry
* `meeseOS/tray => (options?)` - Creates a new [Tray](../tray/README.md) entry
* `meeseOS/clipboard => ()` - APIs for performing [Clipboard](../clipboard/README.md)
* `meeseOS/settings => ()` - APIs for [Settings](../settings/README.md)
* `meeseOS/vfs => ()` - APIs for [VFS](../vfs/README.md)
* `meeseOS/auth => ()` - APIs for [Authentication](../auth/README.md)
* `meeseOS/contextmenu => (options?)` - APIs for [Context Menus](../gui/README.md#contextmenu)
* `meeseOS/dialog => (name, ...args)` - APIs for [Dialogs](../dialog/README.md#usage)
* `meeseOS/dialogs => ()` - APIs for [Custom Dialogs](../dialog/README.md#custom-dialog)
* `meeseOS/dnd => ()` - APIs for performing [Drag-and-Drop](../tutorial/dnd/README.md) operations
* `meeseOS/theme => ()` - APIs for [Themes](../tutorial/theme/README.md#usage)
* `meeseOS/packages => ()` - APIs for [Package Management](https://meeseOS.github.io/meeseOS-manual/api/meeseOS-client/class/src/packages.js~Packages.html)
* `meeseOS/session => ()` - APIs for [Session](https://meeseOS.github.io/meeseOS-manual/api/meeseOS-client/class/src/session.js~Session.html)
* `meeseOS/desktop => ()` - APIs for desktop
* `meeseOS/panels => ()`- APIs for panels

Example:

```javascript
core.make("meeseOS/settings").save();
```

### Server

The server also has some extra methods:

```javascript
// Broadcast an event to all connected users (WebSocket)
core.broadcast("event-name", [1, 2, 3])

// Broadcast an event to a set of users
core.broadcast("event-name", [1, 2, 3], ws => {
  //The original "req" containing session etc
  //ws.upgradeReq

  //The original session data
  //ws._meeseOS_client

  return true;
});

// Broadcast to all alias but with expanded arguments:
core.broadcastAll("event-name", 1, 2 , 3);

// Broadcast to a specific user, with expanded arguments:
core.broadcastUser("username", "event-name", 1, 2 , 3);

// Express server
const app = core.app;

// WebSocket server
const ws = core.ws;

// Session server
const session = core.session;
```

> [info] You can listen for broadcast events in the client with

```javascript
// Client-side example:
core.on("event-name", (a, b, c) => console.log(a, b, c)) // => 1 2 3
```

#### Server Services

These are the default provided services and their signatures:

* `meeseOS/express => ()` - APIs for [Express](../express/README.md)
* `meeseOS/packages => ()` - APIs for Packages
* `meeseOS/vfs => ()` - APIs for [VFS](../vfs/README.md#server-api)
* `meeseOS/fs => ()` - APIs for Filesystem interaction

#### Events

* `init => ()` - Main init
* `meeseOS/core:start => ()` - Core start
* `meeseOS/core:started => ()` - Core started
* `meeseOS/core:ping => (req)` - User pinged the server
* `meeseOS/core:vfs:watch:change => ({mountpoint, target, type})` - VFS watch trigger
* `meeseOS/core:logged-in => (session)` - User logged in
* `meeseOS/core:logging-out => (session)` - User is logging out
