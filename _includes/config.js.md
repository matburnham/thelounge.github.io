## Server settings

### `public`

When set to `true`, The Lounge starts in public mode. When set to `false`,
it starts in private mode.

- A **public server** does not require authentication. Anyone can connect
  to IRC networks in this mode. All IRC connections and channel
  scrollbacks are lost when a user leaves the client.
- A **private server** requires users to log in. Their IRC connections are
  kept even when they are not using or logged in to the client. All joined
  channels and scrollbacks are available when they come back.

This value is set to `true` by default.

### `host`

IP address or hostname for the web server to listen to. For example, set it
to `"127.0.0.1"` to accept connections from localhost only.

This value is set to `undefined` by default to listen on all interfaces.

### `port`

Set the port to listen to.

This value is set to `9000` by default.

### `bind`

Set the local IP to bind to for outgoing connections.

This value is set to `undefined` by default to let the operating system
pick its preferred one.

### `reverseProxy`

When set to `true`, The Lounge is marked as served behind a reverse proxy
and will honor the `X-Forwarded-For` header.

This value is set to `false` by default.

### `maxHistory`

Defines the maximum number of history lines that will be kept in
memory per channel/query, in order to reduce the memory usage of
the server. Setting this to `-1` will keep unlimited amount.

This value is set to `10000` by default.

### `https`

These three settings are used to run The Lounge using encrypted HTTP/2 on
the server side. This will fallback to regular HTTPS if HTTP/2 is not
supported.

The available keys for the `https` object are:

- `enable`
- `key`: Path to the private key file.
- `certificate`: Path to the certificate.

The value of `enable` is set to `false` to disable HTTPS by default, in
which case the other two string settings are ignored.

## Client settings

### `theme`

Set the default theme to serve to new users. They will be able to select a
different one in their client settings among those available.

All available themes are located in the `client/themes/` directory of the
installed application. Read more about how to manage themes at **TODO**.

This value is set to `"themes/example.css"` by default.

### `prefetch`

When set to `true`, The Lounge will try to load thumbnails and site
descriptions from URLs posted in channels and private messages.

This value is set to `false` by default.

### `prefetchMaxImageSize`

When `prefetch` is enabled, images will only be displayed if their file
size does not exceed this limit.

This value is set to `512` kilobytes by default.

### `transports`

Set `socket.io` transports

This value is set to `["polling", "websocket"]` by default.

## Default network

### `defaults`

Specifies default network information that will be used as placeholder
values in the *Connect* window.

The available keys for the `defaults` object are:

- `name`: Name to display in the channel list of The Lounge. This value is
  not forwarded to the IRC network.
- `host`
- `port`: *de facto* standard is 6667 for unencrypted connections and 6697
  for connections encrypted with TLS.
- `password`
- `tls`: Enable TLS connections
- `nick`
- `username`
- `realname`
- `join`: Comma-separated list of channels to auto-join once connected.

This value is set to connect to the official channel of The Lounge on
Freenode by default:

```js
defaults: {
  name: "Freenode",
  host: "chat.freenode.net",
  port: 6697,
  password: "",
  tls: true,
  nick: "lounge-user",
  username: "lounge-user",
  realname: "The Lounge User",
  join: "#thelounge"
},
```

### `displayNetwork`

When set to `false`, network fields will not be shown in the "Connect"
window.

Note that even users cannot access these fields, they can still connect to
other networks using the `/connect` command. See the `lockNetwork` setting
to restrict users from connecting to other networks.

This value is set to `true` by default.

### `lockNetwork`

When set to `true`, users will not be able to modify host, port and TLS
settings and will be limited to the configured network.

It is often useful to use it with `displayNetwork` when setting The
Lounge as a public web client for a specific IRC network.

This value is set to `false` by default.

## User management

### `logs`

User logging must be enabled per user, in their configuration file under
`$LOUNGE_HOME/users/<user>.json`. When enabled, logs will be stored in the
`$LOUNGE_HOME/logs/<user>/<network>/` directory.

The available keys for the `logs` object are:

- `format`: Timestamp format `"YYYY-MM-DD HH:mm:ss"`

- `timezone`: Timezone `"UTC+00:00"`

### `useHexIp`

When set to `true`, users' IP addresses will encoded has hex.

This is done to share the real user IP address with the server for host
masking purposes.

This value is set to `false` by default.

## WEBIRC support

When enabled, The Lounge will pass the connecting user's host and IP to the
IRC server. Note that this requires to obtain a password from the IRC
network that The Lounge will be connecting to and generally involves a lot
of trust from the network you are connecting to.

There are 2 ways to configure the `webirc` setting:

- **Basic**: an object where keys are IRC hosts and values are passwords.
  For example:

  ```json
  {"irc.example.net": "password1", "irc.example.org": "passw0rd"}
  ```

- **Advanced**: an object where keys are IRC hosts and values are functions
  that take three arguments (`client`, `args`, `trusted`) and return an
  object to be directly passed to `irc-framework`. For example:

  ```js
  {"irc.example.net": (client, args, trusted) => ({
    username: "thelounge",
    password: "password1",
    address: args.ip,
    hostname: `webirc/${args.hostname}`
  })}
  ```

This value is set to `null` to disable WEBIRC by default.

## identd and oidentd support

### `identd`

Run The Lounge with `identd` support.

The available keys for the `identd` object are:

- `enable`: When `true`, the identd daemon runs on server start.
- `port`: Port to listen for ident requests.

The value of `enable` is set to `false` to disable `identd` support by
default, in which case the value of `port` is ignored. The default value of
`port` is 113.

### `oidentd`

When this setting is a string, this enables `oidentd` support using the
configuration file located at the given path.

This is set to `null` by default to disable `oidentd` support.

## LDAP support

These settings enable and configure LDAP authentication.

They are only being used in private mode. To know more about private mode,
see the `public` setting above.

The available keys for the `ldap` object are:

- `enable`
- `url`
- `baseDN`
- `primaryKey`

`enable` must be set to `true` or `false`, all other values must be
strings.

The value for `enable` is set to `false` to disable LDAP support by
default, in which case all other values are ignored.

## Debugging settings

The `debug` object contains several settings to enable debugging in The
Lounge. Use them to learn more about an issue you are noticing but be aware
this may produce more logging or may affect connection performance so it is
not recommended to use them by default.

All values in the `debug` object are set to `false`.

### `debug.ircFramework`

When set to true, this enables extra debugging output provided by
[`irc-framework`](https://github.com/kiwiirc/irc-framework), the
underlying IRC library for Node.js used by The Lounge.

### `debug.raw`

When set to `true`, this enables logging of raw IRC messages into each
server window, displayed on the client.
