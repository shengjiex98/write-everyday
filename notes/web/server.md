# Server

## Nginx server

### Server block

### Location block

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