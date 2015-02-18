# Supported tags and respective `Dockerfile` links

-      [`cli`, `latest` (*latest/Dockerfile*)](https://github.com/estebanmatias92/docker-hhvm-improved/blob/master/latest/Dockerfile)
-      [`fastcgi` (*latest/fastcgi/Dockerfile*)](https://github.com/estebanmatias92/docker-hhvm-improved/blob/master/latest/fastcgi/Dockerfile)

# What contains this image?

-      [Custom hhvm image](https://registry.hub.docker.com/u/estebanmatias92/hhvm)
-      [Dockerize](https://github.com/jwilder/dockerize)

# How to use this image

## Traditional way

       docker run -d estebanmatias92/hhvm-improved:fastcgi

With this, you have a hhvm server that runs on port 9000 from the container ip.

## Dockerize way

With Dockerize you can redirect logs to Docker log collector and  easily manage any configuration at runtime through templates and environment variables.

Before start to using it, i recommend you to read the [Jason Wilder’s post](http://jasonwilder.com/blog/2014/10/13/a-simple-way-to-dockerize-applications) for a more in-depth explanation about this tool.

       FROM estebanmatias92/hhvm-improved
       COPY ./config/php.tmpl /var/www/config/
       ENTRYPOINT [“dockerize”, “-template”, “/var/www/config/php.tmpl:/etc/hhvm/php.ini”, “-stdout”, “/var/log/hhvm/access.log”, “-stderr”, “/var/log/hhvm/error.log”, “/usr/local/bin/hhvm”]
       CMD [“--mode”, “server”]

If the template that you copied had some placeholder to replace, you can make usage of it:

       docker build --force-rm -t my-hhvm-image .
       docker run -d -e SEVER_PORT=9001 my-hhvm-image

And you have a hhvm server runnin on port 9001.

In the example above i assume that you made a placeholder for “hhvm.server.port” option (hhvm.server.port={{ .Env.SERVER_PORT }}), but you can create this way whatever environment variable that you want.

# License

View [license information](https://github.com/facebook/hhvm#license) for hhvm.

View [license information](https://github.com/jwilder/dockerize#license) for dockerize.

# User Feedback

## Issues

If you have any problems with or questions about this image, please contact me through a [GitHub issue](https://github.com/estebanmatias92/docker-hhvm-improved/issues).

## Contributing

Every contribution are welcome.
