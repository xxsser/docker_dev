FROM nginx:latest
ADD  nginx.conf      /etc/nginx/nginx.conf
ADD  fastcgi.conf	/etc/nginx/fastcgi.conf
RUN  mkdir /opt/htdocs && mkdir /opt/log && mkdir /opt/log/nginx
RUN  chown -R www-data.www-data /opt/htdocs /opt/log
VOLUME ["/data","/etc/nginx/conf.d"]