# fwpunch: firewall puncher using reverse port mapping over SSH

Install using Arch package:

    cd archpkg && makepkg -si

This will create a fwpunch user and generate an SSH key in
`/var/lib/fwpunch/.ssh/id_rsa`.

On any webserver create an externally accessible file server with the
information about the gateway server in the format:

	user@thegatewayserver port remoteport tunnelport

The `fwpunch` will connect via SSH to `thegatewayserver` on port `port`
and open a tunnel from the gateway server on port `tunnelport` to the port
`remoteport` on the host that's punching out.

On the host running `fwpunch`, configure the URL to the webserver in
`/etc/fwpunch/thegateway.conf`:

	HQ_URL=http://thewebserver/gatewayinfo.txt

On the gateway server create a user account and add the above-generated key to
its `~/.ssh/authorized_keys`.

Start the daemon as a systemd service:

	sudo systemctl start fwpunch@thegateway

To access the punching host from the gateway host:

	ssh -o 'Port tunnelport` punchinguser@localhost

where `tunnelport` is the actual number configured above.
