# NPS Intranet Penetration Tool

NPS is a lightweight, high-performance, and powerful **intranet penetration** proxy server. It supports **TCP and UDP traffic forwarding**, any **TCP and UDP** upper-layer protocols (accessing intranet websites, local payment interface debugging, SSH access, remote desktop, intranet DNS resolution, etc.). In addition, it also supports **intranet HTTP proxy, intranet SOCKS5 proxy, P2P**, and features a powerful web management interface.

## Quick Start Guide

Ensure it has been compiled, then run the following startup commands in the project root directory:

### 1. Start Server (NPS)

You can run the following command directly in the project directory to start NPS:

```bash
# Run server in the foreground, outputting logs to the terminal
./nps

# Or: Run server in the background and output logs to nps_run.log
./nps > nps_run.log 2>&1 &
```

> **Web Management Console**
> Address: http://127.0.0.1:28080
> Default Username: `admin`
> Default Password: `123`

### 2. Start Client (NPC)

Open the Web Console (http://127.0.0.1:28080) in your browser and follow these steps to obtain a unique key:

1. Log in to the Web Console (default account: `admin` / `123`).
2. Click **"Clients"** in the left menu bar.
3. Click the **"Add"** button on the page, fill in a remark name, and click save.
4. In the newly appeared client record, click the **"+"** sign on the far left (or view the **"Verification Key (vkey)"** column).
5. Copy the client startup command displayed in the panel or just copy the `vkey`.

```bash
# Run this startup command on the target device to be penetrated
./npc -server=127.0.0.1:8024 -vkey=<Your copied vkey> -type=tcp
```

### 3. How to Stop Services

If you started using the foreground command, simply press `Ctrl + C` in the terminal to stop.
If you used the background run `&`, you can use process management commands to stop:

```bash
# Stop NPS Server
pkill nps

# Stop NPC Client
pkill npc
```

## Function Usage Guide

Assuming your local machine (running the `nps` server) is both the server and the client, and you want to expose a local service running on this machine to external access.

### Method 1: TCP Tunnel Penetration (For SSH, Database, Local Web Dev, etc.)

If you want to access the local service directly via `IP:Port`, configure a TCP tunnel.

1. **Create a client in the Web Console** and get the `vkey`.
2. **Start NPC (client) on the local machine**: `./npc -server=127.0.0.1:8024 -vkey=YOUR_VKEY -type=tcp &`
3. **Configure TCP tunnel in the Web Console**:
   *   Click **"TCP Tunnel"** -> **"Add"** in the left menu.
   *   **Client ID**: Fill in your client ID.
   *   **Server Port**: Enter a port you want to be accessed externally (e.g., `9090`).
   *   **Target (IP:Port)**: Enter the local service address and port (e.g., `127.0.0.1:3000`).

### Method 2: Domain Name Resolution (Host) (For HTTP/HTTPS Websites)

Access different Web website projects in the intranet through different domain names.

1. Click the **"Host"** button in the client list (or **"Domain Resolution"** in the left menu) -> **"Add"**.
2. **Host (Domain)**: Fill in the domain you plan to use for access (e.g., `test.yourdomain.com`).
3. **Target (IP:Port)**: Enter your local Web service address (e.g., `127.0.0.1:8080`).
4. **How to access**: Visit `http://test.yourdomain.com:18080` from an external network to penetrate to the local `8080` website.

### Advanced Features

*   **UDP Tunnel**: Suitable for game servers/DNS.
*   **SOCKS5 Proxy**: Suitable for VPN effects, using the intranet network remotely.
*   **File Access**: Share local folder files temporarily to the external network.
*   **P2P Connection**: Direct high-speed connection between two intranet devices.
*   **Secret Proxy**: Extremely secure, for when you do not want to expose any ports publicly.
