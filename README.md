# Green Tunnel
<p align="center">
    <img src="assets/logo.png" alt="green tunnel logo" width="200">
</p>
<p align="center">
    <img src="https://img.shields.io/github/license/SadeghHayeri/GreenTunnel.svg?color=Green&style=for-the-badge"> <img src="https://img.shields.io/github/repo-size/SadeghHayeri/GreenTunnel.svg?color=Green&style=for-the-badge"> <img src="https://img.shields.io/discord/707464295021019197?color=Green&style=for-the-badge">
</p>

GreenTunnel bypasses DPI (Deep Packet Inspection) systems found in many ISPs (Internet Service Providers) which block access to certain websites.

<p align="center">
    <img src="assets/demo.gif" alt="green tunnel demo" style="margin-top: 20px;">
</p>

## How to use
### Graphical user interface (GUI)
You can simply choose the suitable installation for your OS in the [releases](https://github.com/SadeghHayeri/GreenTunnel/releases "releases") section.

### Command-line interface (CLI)
You can install GreenTunnel using [npm](https://www.npmjs.org/ "npm"):
```
$ npm i -g green-tunnel
```

or using [snap](https://snapcraft.io) (edge version):
```
sudo snap install --edge green-tunnel --devmode
```


after installation you can run it using `gt` or `green-tunnel` commands.

```
$ gt --help
Usage: green-tunnel [options]
Usage: gt [options]

Options:
  --help, -h      Show help                                            [boolean]
  --version, -V   Show version number                                  [boolean]
  --ip            ip address to bind proxy server[string] [default: "127.0.0.1"]
  --https-only    Block insecure HTTP requests        [boolean] [default: false]
  --port          port address to bind proxy server     [number] [default: 8000]
  --dns-type               [string] [choices: "https", "tls"] [default: "https"]
  --dns-server        [string] [default: "https://cloudflare-dns.com/dns-query"]
  --dns-ip        IP address for unencrypted DNS  [string][default: "127.0.0.1"]
  --dns-port      Port for unencrypted DNS                [number] [default: 53]
  --silent, -s    run in silent mode                  [boolean] [default: false]
  --verbose, -v   debug mode                              [string] [default: ""]
  --system-proxy  automatic set system-proxy           [boolean] [default: true]

Examples:
  gt
  gt --ip 127.0.0.1 --port 8000 --https-only
  gt --dns-server https://doh.securedns.eu/dns-query
  gt --verbose 'green-tunnel:proxy*'

ISSUES:  https://github.com/SadeghHayeri/GreenTunnel/issues
```

for debug use verbose option:
```
$ green-tunnel --verbose 'green-tunnel:*'
```

### Docker
```
$ docker run -p 8000:8000 sadeghhayeri/green-tunnel
```
> **envs**
* PORT
* HTTPS-ONLY
* VERBOSE
* SILENT
* DNS_TYPE
* DNS_SERVER

usage:
```
$ docker run -e 'PORT=1000' -p 8000:1000 sadeghhayeri/green-tunnel
```

#### On Raspberry Pi
```
$ docker run -p 8000:8000 sadeghhayeri/green-tunnel:arm
```

If you want to make container keep running when reboot:
```
$ docker run -d --restart unless-stopped -p 8000:8000 sadeghhayeri/green-tunnel:arm
```

Please make sure port `8000` is not blocked on Raspberry Pi firewall. (`sudo ufw allow 8000 comment Green-Tunnel`)

To use it on your other device, set http proxy to ```<Raspberry Pi IP Address>:<PORT>```. (PORT = `8000`)

### Tested on
- MacOS Catalina with node 12
- Ubuntu 18.04 with node 8
- Windows 10 with node 8


## FAQ
> **How does it work?**
###### HTTP
There are gaps in providers in DPI.  They happen from what the DPI rules write for ordinary user programs, omitting all possible cases that are permissible by standards.  This is done for simplicity and speed.
Some DPIs cannot recognize the HTTP request if it is divided into TCP segments.  For example, a request of the form

```
GET / HTTP/1.0`
Host: www.youtube.com
...
```
we send it in 2 parts: first comes `GET / HTTP/1.0 \n Host: www.you` and second sends as `tube.com \n ...`. In this example, ISP cannot find blocked word **youtube** in packets and you can bypass it!

###### HTTPS
Server Name Indication (SNI) is an extension to TLS (Transport Layer Security) that indicates the actual destination hostname a client is attempting to access over HTTPS. For this Web Filter feature, SNI hostname information is used for blocking access to specific sites over HTTPS. For example, if the administrator chooses to block the hostname **youtube** using this feature, all Website access attempts over HTTPS that contain **youtube** like **www.youtube.com** in the SNI would be blocked. However, access to the same hostname over HTTP would not be blocked by this feature. GreenTunnel tries to split first **CLIENT-HELLO** packet into small chunks and ISPs can't parse packet and found SNI field so bypass traffic!

###### DNS
When you enter a URL in a Web browser, the first thing the Web browser does is to ask a DNS (Domain Name System) server, at a known numeric address, to look up the domain name referenced in the URL and supply the corresponding IP address.
If the DNS server is configured to block access, it consults a blacklist of banned domain names. When a browser requests the IP address for one of these domain names, the DNS server gives a wrong answer or no answer at all.
GreenTunnel use [DNS over HTTPS](https://en.wikipedia.org/wiki/DNS_over_HTTPS "doh (DNS over HTTPS)") and [DNS over TLS](https://en.wikipedia.org/wiki/DNS_over_TLS "DNS over TLS") to get real IP address and bypass DNS Spoofing.

## Development notes
GreenTunnel is an open-source app and I really appreciate other developers adding new features and/or helping fix bugs. If you want to contribute to GreenTunnel, you can fork this repository, make the changes and create a pull request.

However, please make sure you follow a few rules listed below to ensure that your changes get merged into the main repo. The rules listed below are enforced to make sure the changes made are well-documented and can be easily kept track of.

- ⇄ Pull requests and ★ Stars are always welcome.
- For bugs and feature requests, please create an issue.
- Make sure your pull request has an informative title. You should use prefixes like `ADD:`, `FIX:`, etc at the start of the title which describes the changes followed by a one-line description of the changes. Example: ADD: Added a new feature to GreenTunnel
- Commits in your fork should be informative, as well. Make sure you don't combine too many changes into a single commit.


