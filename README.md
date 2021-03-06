# PHP Web Development Environment based on Docker, by [FEROX](https://ferox.yt)

Provides a modern web development environment for PHP developpers based on Docker. It features:

* [Adminer](https://www.adminer.org/)
* [MailDev](https://github.com/maildev/maildev)
* [Traefik](https://docs.traefik.io/)
* [XHGui](https://github.com/frxyt/docker-xhgui-dev)

## Install

1. Be sure to have [Docker](https://hub.docker.com/?overlay=onboarding) & [docker-compose](https://docs.docker.com/compose/install/) installed
1. `git clone https://github.com/frxyt/webdevenv-php`
1. `cd webdevenv-php`
1. `docker network create webdevenv`
1. `docker-compose up -d`

### Debian-based Linux distributions

You need to resolve `*.localhost` domains to `127.0.0.1`, unfortunately, the good old `dnsmasq` doesn't work out-of-the-box now because of `systemd`, here are some steps to make it working:

1. `sudo apt-get install dnsmasq`
1. `sudo sed -i 's/^#\?DNS=.*$/DNS=127.0.0.53/' /etc/systemd/resolved.conf`
1. `sudo sed -i 's/^#\?listen-address=.*$/listen-address=127.0.0.53/' /etc/dnsmasq.conf`
1. `echo 'address=/localhost/127.0.0.1'  | sudo tee -a /etc/dnsmasq.conf`

1. `sudo sed -i 's/^#\?IGNORE_RESOLVCONF=.*$/IGNORE_RESOLVCONF=yes/' /etc/default/dnsmasq`
1. `sudo service restart dnsmasq`
1. `sudo service restart systemd-resolved`

### Mac OS & Windows

You don't need any extra steps as these OS resolve automatically `*.localhost` domains to `127.0.0.1` !

## Update

1. `git pull`
1. `docker-compose down`
1. `docker-compose up -d`

Or just run: `./update` (Mac OS & Linux), `bash ./update` (Windows)

## Usage

### Accessing tools

* Adminer: http://adminer.webdevenv.localhost
* MailDev: http://maildev.webdevenv.localhost
* Traefik: http://traefik.webdevenv.localhost
* XHGui: http://xhgui.webdevenv.localhost

### Use it in your `docker-compose.yml` files

1. Declare `webdevenv` network:
   ```yaml
   networks: 
     webdevenv:
       external: 
         name: webdevenv
   ```
1. Add `webdevenv` network to database containers, containers which need to send emails and containers which need to be accessed over HTTP:
   ```yaml
   networks: 
     - webdevenv
   ```
1. Add Traefik labels on containers which needs to be accessed over HTTP:
   ```yaml
   labels: 
     - "traefik.enable=true"
     - "traefik.http.routers.myproject-nginx.entrypoints=http"
     - "traefik.http.routers.myproject-nginx.rule=Host(`nginx.myproject.localhost`)"
     - "traefik.http.services.myproject-nginx.loadbalancer.server.port=80"
   ```

## License

This project and images are published under the [MIT License](LICENSE).

```
MIT License

Copyright (c) 2019,2020 FEROX YT EIRL, www.ferox.yt <devops@ferox.yt>
Copyright (c) 2019,2020 Jérémy WALTHER <jeremy.walther@golflima.net>

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```