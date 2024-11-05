# Tailscale NordVPN Exit Node

This is a simple Docker setup that runs a Tailscale node, routes all traffic through a NordVPN connection, and advertises the Tailscale node as an exit node.

It uses two Docker containers: one for Tailscale and one for NordVPN. The Tailscale container is configured to route all traffic through the NordVPN container. For the NordVPN container, it uses the great [Gluetun VPN container](https://github.com/qdm12/gluetun), and for the Tailscale container, it uses the official [Tailscale container](https://tailscale.com/kb/1282/docker/).

Check their respective documentation for more information on how to configure them. This is more meant to be an example and could be used with [most other VPN providers](https://github.com/qdm12/gluetun-wiki/tree/main/setup/providers), too.

## Prerequisites

- Docker
- Docker Compose

## Usage

1. **Clone the repository and navigate to the directory:**
   ```sh
   git clone https://github.com/fnep/tailscale-nordvpn
   cd tailscale-nordvpn
   ```

2. **Copy the `.env.example` to `.env` and fill in the missing values:**
   - To get NordVPN credentials, you need to have a subscription. You can find the credentials in the NordVPN dashboard under `Manual setup`.
   - To get the Tailscale auth key, you need to have a Tailscale account. You can create a new auth key in the Tailscale console under `Settings -> Keys -> Generate auth key`.

3. **Run the containers:**
   ```sh
   docker compose up -d
   ```

4. **View the logs of the containers:**
   ```sh
   docker compose logs -f
   ```
   Press `Ctrl+C` to exit.

5. **Enable the exit node in the Tailscale console:**
   - Click on the three dots menu next to the node and select `Edit route settings`.
   - Set the checkbox for `Use as exit node` and save.

> [!TIP]
> The Tailscale auth key will last for at most 90 days. After that, you need to generate a new one and update the `.env` file.
> To avoid this, you can disable the key expiration in the Tailscale console for this particular node by clicking on the three dots menu next to the node and selecting `Disable key expiry`.


To stop the containers, run:
```sh
docker compose down
```

## Configuration

- **`.env` file:**
  - `NORDVPN_OPENVPN_USER`: Your NordVPN manual setup username.
  - `NORDVPN_OPENVPN_PASSWORD`: Your NordVPN manual setup password.
  - `NORDVPN_SERVER_COUNTRIES`: The country for the NordVPN server (e.g., `Germany`).
  - `TAILSCALE_HOSTNAME`: The name your node should appear as in the Tailscale console.
  - `TAILSCALE_AUTHKEY`: The auth key for Tailscale.
