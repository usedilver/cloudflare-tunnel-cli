# cftunnel

CLI to manage Cloudflare Tunnels in a single command. Automates the full lifecycle: **create tunnel + configure DNS + generate config + run**.

> No more manual steps. One command to create, one to run.

## Why?

Setting up a named tunnel with `cloudflared` requires multiple steps:

1. `cloudflared tunnel create name`
2. `cloudflared tunnel route dns name hostname`
3. Manually create a `.yml` file with the tunnel ID, credentials path, and ingress rules
4. `cloudflared tunnel --config file.yml run`

**cftunnel** reduces this to:

```bash
cftunnel create my-api example.com 3000
cftunnel run my-api
```

## Installation

### Prerequisites

1. Install **cloudflared**:

   ```bash
   brew install cloudflared
   ```

   Or see [other installation methods](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/downloads/).

2. Log in to your Cloudflare account:

   ```bash
   cloudflared tunnel login
   ```

   This will open a browser window where you select the domain you want to use with your tunnels.

### Option 1: Homebrew

```bash
brew tap usedilver/tap
brew install cftunnel
```

### Option 2: curl

```bash
curl -fsSL https://raw.githubusercontent.com/usedilver/cloudflare-tunnel-cli/main/cftunnel -o ~/.local/bin/cftunnel
chmod +x ~/.local/bin/cftunnel
```

> Make sure `~/.local/bin` is in your `PATH`. If not, add this to your shell profile:
>
> ```bash
> export PATH="$HOME/.local/bin:$PATH"
> ```

## Usage

### Create a tunnel

```bash
cftunnel create my-api example.com 3000
```

In a single step, this:

- Creates the tunnel in Cloudflare (or reuses an existing one)
- Configures the DNS route (`my-api.example.com`)
- Generates the `.yml` configuration file

### Run a tunnel

```bash
cftunnel run my-api
```

### Interactive selector

```bash
cftunnel run
```

Shows all configured tunnels and lets you choose which one to start:

```
Available tunnels:

   1) my-api              https://my-api.example.com             :3000
   2) my-web              https://my-web.example.com             :8080
   3) testing             https://testing.example.com            :5000

Select tunnel (1-3) or 'q' to quit:
```

### List tunnels

```bash
cftunnel list
```

### Change port

```bash
cftunnel port my-api 8080
```

### Edit configuration

```bash
cftunnel edit my-api
```

### Delete a tunnel

```bash
cftunnel delete my-api
```

Removes the tunnel from Cloudflare, deletes the config file and credentials.

### View tunnels in Cloudflare

```bash
cftunnel status
```

## Commands

| Command                                  | Description                                 |
| ---------------------------------------- | ------------------------------------------- |
| `cftunnel create <name> <domain> <port>` | Create tunnel + DNS route + config          |
| `cftunnel run [name]`                    | Run a tunnel (interactive if no name given) |
| `cftunnel list`                          | List locally configured tunnels             |
| `cftunnel delete <name>`                 | Delete tunnel and cleanup files             |
| `cftunnel edit <name>`                   | Open tunnel config in your editor           |
| `cftunnel port <name> <port>`            | Change a tunnel's port                      |
| `cftunnel status`                        | Show tunnels registered in Cloudflare       |
| `cftunnel help`                          | Show help                                   |

## Environment variables

| Variable          | Default          | Description                        |
| ----------------- | ---------------- | ---------------------------------- |
| `CLOUDFLARED_DIR` | `~/.cloudflared` | Directory where configs are stored |

## Compatibility

- macOS (auto-opens browser with `open`)
- Linux (auto-opens browser with `xdg-open`)

## Contributing

Contributions are welcome! Feel free to open issues and pull requests.

## License

[MIT](LICENSE)
