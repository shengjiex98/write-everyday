# Server

## Nginx server

### Server block

### Location block

#### Redirect

Use `return 301` to redirect from one location block to another. For example:
```
location /plotting/ {
    try_files $uri $uri/ $uri.html /404.html;
}
location /graph/ {
    return 301 /plotting/;
}
```
will redirect `example.com/graph/hello` to `example.com/plotting`. In other words, anything that comes after `/graph` will be ignored.

## HTTPS

[Use certbot with nginx+ubuntu](https://certbot.eff.org/instructions?ws=nginx&os=ubuntufocal). This uses snap. In short:

```bash
$ sudo snap install core; sudo snap refresh core
$ sudo apt remove certbot
$ sudo snap install --classic certbot
$ which certbot
/usr/bin/certbot
$ sudo certbot --nginx # or any other server
$ sudo certbot renew --dry-run
```