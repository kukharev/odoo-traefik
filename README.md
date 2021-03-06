# odoo-traefik

Odoo + Traefik (replacing the Nginx reverse proxy)


## Important note
The docker image wbsouza/odoo:11.0 contains everything to run Odoo (including the PostgreSQL).
It's not recommended to run this image in production, it was build only for test purpose, to
demonstrate a potential inconsistence using Traefik as a proxy for Odoo. Feel free to customize or
make your own Odoo image, the source code of the image is available on [https://github.com/wbsouza/odoo-docker](https://github.com/wbsouza/odoo-docker).

*** The database is going to be re-created if you remove the file `volumes/odoo/conf/.initialized`. ***

## Initial setup

1. Access your domain provider and add the following entries to your domain (`A` or `CNAME`)
  - `traefik.mycompany.io`
  - `odoo.mycompany.io`

2. Create a bridge network to be used by the containers
  - `docker create network web`

3. Copy the file .env.sample to .env and change the domain name and the email for the LetsEncript certificate

4. Execute `docker-compose up`

5. Login with the default credentials (admin/admin)

6. Install the CRM module (or any other backend module)

7. Stop the odoo container
`docker stop odoo`

8. Change the configuration file `volumes/odoo/conf/odoo.conf` append the following parameters in the end of the file:
```
proxy_mode = True
workers = 4
```

9. Login again with the default credentials

10. Check the log files as well the browser looking for JavaScript errors, it should be working without any problem


## Throubleshooting (knowed issue)
1) If you want create volumes like in the `odoo.conf.sample` file, make sure to give the right permissions or change the owner
in the host filesystem volumes, because the container will run with an unprivileged user (`odoo, UID=9100, GID=9100`).

`chown -fR '9100:9100' volumes/odoo`

