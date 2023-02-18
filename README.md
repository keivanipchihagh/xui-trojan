# xui-trojan
I'll explain how to setup the popular "*Trojan+XTLS+DNS+TCP*" stack that has worked seamlessly for months.
> **Note**
> You can also use other protocols like **VLESS** and **VMESS**. To my experience, **VLESS** is faster while **Trojan** is the more secure.

## Table of Contents
- 💫 First things first
- 🪖 (Optional) Hold on to your Firewalls!
- 🐳 Run it with Docker!
- 🚀 Let the dashboard, Begin!
- 🔐 Secure your dashboard with HTTPS
- 📬 Create Inbounds / Benchmark
- ☃️ Build up from here


## 💫 First things first
1. Get a Domain, buy a Server and create a [Cloudflare](https://cloudflare.com/) account.
2. Set NS records that points your domain to your server. Changes can take 1-24 hours to apply, so seat back and track domain availability with [dnshealth](https://dnschecker.org/).
3. Map a subdomain to server IP (Leave *proxied* unchecked for now!)

## 🪖 (Optional) Hold on to your Firewalls!
I always enjoy the extra security on my servers. If you suffer from ADHD like I do (*LOL!*), there is an script for you! Run `setup-ufw.sh` to configure a minimal Firewall with default policies.

> **Warning**
> If you enable Firewall, you have to allow each X-UI inbound through it!

## 🐳 Run it with Docker! 
I'm going to use [Docker](https://www.docker.com/), because it's clean and leaves no trace. You can remove the container like nothing ever happened. Install it by running the `setup-docker.sh` script.

> **Note**
> The script will install *Docker Engine*, *docker-compose* and add them to sudo group.

## 🚀 Let the dashboard, Begin!
1. There is a file called `.env.template` which containes placeholders for variables that you must chnage. Finally rename the file to `.env`.
2. Make sure nothing is blocking port 80 until the end of this section. If there is, stop it temporarily.
3. Run the `build.sh` script which generates a SSL certificate using **certbot** and deploys the X-UI container.
4. Access your dashboard via `<SERVER-IP>:<DASHBOARD-PORT>` where the default dashboard port is `54321`.

> **Warning**
> The default username and password for the dashboard are "**admin**". Change them immediately!

## 🔐 Secure your dashboard with HTTPS
You can always access your dashboard via `<SERVER-IP>:<DASHBOARD-PORT>`, but that leaves you unprotected. Do the followings to establish a HTTPS connection with your dashboard:
1. On your [Cloudflare](https://cloudflare.com/) account, set the *SSL/TLS* level to `strict` and beyond.
2. Navigate to **Panel Settings** and change these fields as followed:
    - Panel certificate.crt file path: `/root/certs/fullchain.pem`
    - Panel private.key file path: `/root/certs/privkey.pem`

Now you can access your dashboard via `<DOMAIN>:<DASHBOARD-PORT>` which falls behind HTTPS.

## 📬 Create Inbounds / Benchmark
Create inbounds for your clients. Note the followings:
- Stick with **XTLS** rather than **TLS** whenever possible.
- Always add *"Certificate.crt"* and *"Private.key"* paths like the previous section.
- **TCP** is faster while **Websocket** can be configured with CDN.
- Uncheck *"Disable Insecure Connections"* for very old devices, keep it on otherwise. (Do it client side!)

> **Note**
> If you have an active Firewall, you need to add each inbound port to it after creation!

## ☃️ Build up from here
There are still a lot you could do to reinforce your VPN. For instance:
- Not working on some ISPs? Could use **IPv6** if you know how😎. Nobody will look for you there...
- Want to hide your server IP? Hide behind a CDN.

## 🤝 Issues and Contributions
Feel free to ask questions via [issue](https://github.com/keivanipchihagh/xui-trojan/issues/new) and add features by opening a [pull request](https://github.com/keivanipchihagh/xui-trojan/pulls).
