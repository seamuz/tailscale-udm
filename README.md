# Tailscale on Unifi Dream Machine
This repo contains the scripts necessary to install and run a [tailscale](https://tailscale.com)
instance on your [Unifi Dream Machine](https://unifi-network.ui.com/dreammachine) (UDM/UDM Pro).
It does so by piggy-backing on the excellent [boostchicken/udm-utilities](https://github.com/boostchicken/udm-utilities)
to provide a persistent service and runs using Tailscale's usermode networking feature.

## Instructions
### Install Tailscale
1. Follow the steps to install the boostchicken `on-boot-script` [here](https://github.com/boostchicken/udm-utilities/tree/master/on-boot-script).
2. Run the `install.sh` script to install `tailscale` and the startup script on your UDM.
   
   ```sh
   curl -sSL https://raw.githubusercontent.com/seamuz/tailscale-udm/main/install.sh | sh
   ```
3. Follow the on-screen steps to configure `tailscale` and connect it to your network.
4. Confirm that `tailscale` is working by running `/mnt/data/tailscale/tailscale status`

### Tailscale as subnet router

```sh
/mnt/data/tailscale/tailscale up --advertise-routes=10.0.0.0/24,10.0.1.0/24
```
Replace the subnets in the example above with the right ones for your network. Both IPv4 and IPv6 subnets are supported.

### Tailscale as exit node

```sh
/mnt/data/tailscale/tailscale up --advertise-exit-node
```

### Upgrade Tailscale to version 1.14.0
Upgrading can be done by running the upgrade script below.

```sh
curl -sSL https://raw.githubusercontent.com/seamuz/tailscale-udm/main/upgrade.sh | sh
```

### Remove Tailscale
To remove Tailscale automatically, you can run the following command, 
   
```sh
curl -sSL https://raw.githubusercontent.com/seamuz/tailscale-udm/main/uninstall.sh | sh
```

or run the steps below manually.

1. Kill the `tailscaled` daemon.
   
   ```sh
   ps | grep tailscaled
   kill <PID>
   ```
2. Remove the boot script using `rm /mnt/data/on_boot.d/10-tailscaled.sh`
3. Have tailscale cleanup after itself using `/mnt/data/tailscale/tailscaled --cleanup`.
4. Remove the tailscale binaries and state using `rm -Rf /mnt/data/tailscale`.

## Contributing
There are clearly lots of folks who are interested in running Tailscale on their UDMs. If
you're one of those people and have an idea for how this can be improved, please create a
PR and we'll be more than happy to incorporate the changes.
