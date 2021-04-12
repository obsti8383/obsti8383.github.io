---
layout: post
title:  "Manual Installation of Traefik Proxy"
date:   2021-04-10 08:41:10 +0200
categories: update
---

If you don't want to use docker containers (using docker is pretty much the standard with Traefik), you can also do a manual install on a Debian Linux system.

Credits: taken from the following gist with minor adaptions: <https://gist.github.com/ubergesundheit/7c9d875befc2d7bfd0bf43d8b3862d85>

Download the corresponding binary (e.g. linux-amd64) from <https://github.com/traefik/traefik/releases>.

Put the traefik binary in the system wide binary directory and give it appropriate ownership and permissions:

    sudo cp traefik/traefik /usr/local/bin
    sudo chown root:root /usr/local/bin/traefik
    sudo chmod 755 /usr/local/bin/traefik

Give the traefik binary the ability to bind to privileged ports (e.g. 80, 443) as a non-root user:

    sudo setcap 'cap_net_bind_service=+ep' /usr/local/bin/traefik

Set up the user, group, and directories that will be needed (verify that group id / user id do not exist yet or let groupadd/useradd choose the id by leaving it out from command):

	sudo groupadd -g 321 traefik
    sudo useradd -g traefik --no-user-group --home-dir /var/www --no-create-home --shell /usr/sbin/nologin --system --uid 321 traefik

	sudo mkdir /etc/traefik
	sudo mkdir /etc/traefik/config
	sudo mkdir /etc/traefik/acme
	sudo chown -R root:root /etc/traefik
	sudo chown -R traefik:traefik /etc/traefik/config
	sudo mkdir /var/log/traefik
	sudo chown -R traefik:traefik /var/log/traefik

Place your traefik configuration file (`traefik.toml`, example below) in the proper directory and give it appropriate ownership and permissions:

	sudo cp traefik/traefik.toml /etc/traefik/
	sudo chown root:root /etc/traefik/traefik.toml
	sudo chmod 644 /etc/traefik/traefik.toml
	
	cp /root/traefik/acme.json /etc/traefik/acme/
	cp /root/traefik/config/* /etc/traefik/config/
	sudo chown -R traefik:traefik /etc/traefik/acme

Adjust the config directory and logfile location in traefik.toml:

	[global]
	  checkNewVersion = true
	  sendAnonymousUsage = false
	[entryPoints]
	  [entryPoints.web]
	    address = ":80"
	  [entryPoints.websecure]
	    address = ":443"
	[certificatesResolvers.myresolver.acme]
	  email = "youremail@youremaildomain.de"
	  storage = "/etc/traefik/acme/acme.json"
	  keyType = "RSA2048"
	  [certificatesResolvers.myresolver.acme.httpChallenge]
	    entryPoint = "web"
	[log]
	  level = "ERROR"
	  filePath = "/var/log/traefik/traefik.log"
	[accessLog]
	  filePath = "/var/log/traefik/access_log.txt"
	[api]
	[providers]
	  [providers.file]
	    directory = "/etc/traefik/config"
	    watch = true
	[serversTransport]
	  insecureSkipVerify = true


Install the systemd service unit configuration file, reload the systemd daemon, and start traefik:

First create `traefik.service`:

	[Unit]
	Description=traefik proxy
	After=network-online.target
	Wants=network-online.target systemd-networkd-wait-online.service
	
	[Service]
	Restart=on-abnormal
	
	; User and group the process will run as.
	User=traefik
	Group=traefik
	
	; Always set "-root" to something safe in case it gets forgotten in the traefikfile.
	ExecStart=/usr/local/bin/traefik --configfile=/etc/traefik/traefik.toml
	
	; Limit the number of file descriptors; see `man systemd.exec` for more limit settings.
	LimitNOFILE=1048576
	
	; Use private /tmp and /var/tmp, which are discarded after traefik stops.
	PrivateTmp=true
	; Use a minimal /dev (May bring additional security if switched to 'true', but it may not work on Raspberry Pi's or other devices, so it has been disabled in this dist.)
	PrivateDevices=false
	; Hide /home, /root, and /run/user. Nobody will steal your SSH-keys.
	ProtectHome=true
	; Make /usr, /boot, /etc and possibly some more folders read-only.
	ProtectSystem=full
	; … except /etc/ssl/traefik, because we want Letsencrypt-certificates there.
	;   This merely retains r/w access rights, it does not add any new. Must still be writable on the host!
	ReadWriteDirectories=/etc/traefik/acme
	
	; The following additional security directives only work with systemd v229 or later.
	; They further restrict privileges that can be gained by traefik. Uncomment if you like.
	; Note that you may have to add capabilities required by any plugins in use.
	CapabilityBoundingSet=CAP_NET_BIND_SERVICE
	AmbientCapabilities=CAP_NET_BIND_SERVICE
	NoNewPrivileges=true
	
	[Install]
	WantedBy=multi-user.target

Enable Traefik system service and start Traefik:

	sudo cp /path/to/traefik.service /etc/systemd/system/
	sudo chown root:root /etc/systemd/system/traefik.service
	sudo chmod 644 /etc/systemd/system/traefik.service
	sudo systemctl daemon-reload
	sudo systemctl start traefik.service

Have the Traefik service start automatically on boot if you like:

	sudo systemctl enable traefik.service
