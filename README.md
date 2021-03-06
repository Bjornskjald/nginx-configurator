# nginx-configurator
Simple JSON-based Nginx configurator

SSL forced by default (using Certbot)

**Tested only on Ubuntu (18.04 and 19.04)**, but should work on other platforms.

# Installation
- Fork/clone this repo
- Add your config.json file
- Make sure you have following tools in your PATH
  * Python 3.6
  * Certbot
  * Docker with docker-compose
- Run `update.py`

# Configuration
|   Keyword   | Description | Examples |
|  ---------  | ----------- | -------- |
|     stat    | A static directory with optional custom directory<br>Default: `/var/www/domain.tld` | `stat`<br>`stat /var/website` |
|    index    | Like `stat`, but with autoindexing | `index`<br>`index /var/website` |
|   angular   | Like `stat`, but with fallback to /index.html | `angular`<br>`angular /var/www/website/dist` |
|   redirect  | `301` redirect to specified URL | `redirect https://google.com` |
|    proxy    | Reverse proxy with port/IP and optional path | `proxy 1122`<br>`proxy 1234/path`<br>`proxy https://github.com/` |
| nohostproxy | Same as `proxy`, but without Host header | ^ |
|   custom    | Specifies a file which contains location handler | `custom ./custom-test.conf` |
|    cache    | Used only with other keywords, specifies Nginx cache | `stat; cache custom-name`<br>`proxy https://assets.domain.tld; cache assets` |
|     auth    | Used only with other keywords, specifies .htaccess-compatible file in auth/ directory | `stat; auth test-auth` |

You can temporarily disable domains by prefixing them with `_`

**Example config.json file**
```json
{
  "domain.tld": {
    "/": "proxy 1122",
    "/location": "proxy 1122/another-location",
    "/live-update": "wsproxy 1234"
  },
  "example.com": "nohostproxy https://bjornskjald.github.io"
}
```
