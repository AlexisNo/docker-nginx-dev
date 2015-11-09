# Nginx docker image for development environment

A Docker Nginx image with some useful packages for development environments.
Do not use it for production.


## Common usage

This image should be used as a basic image for any project.

* Create virtualhost for the development environment.
* Create a Dockerfile with your project dependencies and add the virtualhost to it's configuration.
* Create you own image with `docker build` or `docker-compose build`.

Run your new image with a command similar to this:

    docker run -d -p 80:80 -p 443:443 -v /path/to/your/project/sources:/var/www/project-name -v /path/to/project/data/nginx/logs:/var/log/nginx your-image-tag


## Self signed certificates

The image comes with a `gencert` command to generate self signed certificates.

Usage in child Dockerfile:

    RUN gencert <domain>

VirtualHost configuration:

  server {
      listen 80;
      listen 443 ssl;

      ssl_certificate     /etc/ssl/certs/<domain>.crt;
      ssl_certificate_key /etc/ssl/private/<domain>.key;

      server_name <domain>;

      # Complete with your configuration
      # ...
    }

Replace `<domain>` with the hostname you use for the development environment.
