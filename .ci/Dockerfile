FROM openresty/openresty:1.25.3.1-alpine as NGINX
FROM python:3.10-alpine
ENV LANG=C.UTF-8
RUN addgroup -g 101 -S nginx     && adduser -S -D -H -u 101 -h /var/cache/nginx -s /sbin/nologin -G nginx -g nginx nginx  
COPY --from=NGINX /etc/nginx /etc/nginx
COPY --from=NGINX /usr/local/openresty/ /usr/local/openresty/
COPY --from=NGINX /usr/lib/ /usr/lib/
COPY --from=NGINX /var/run/openresty/ /var/run/openresty/
ENTRYPOINT ["/docker-entrypoint.sh"] 
CMD ["/usr/local/openresty/bin/openresty","-g", "daemon off;"]
ENV PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/local/openresty/luajit/bin:/usr/local/openresty/nginx/sbin:/usr/local/openresty/bin
ADD docker-entrypoint.sh /
ADD /html/ /usr/local/openresty/nginx/html/
RUN apk add --no-cache uwsgi
RUN pip install flask requests gevent && rm -rf /root/.cache/pip
RUN ln -s /usr/local/openresty/nginx/conf/nginx.conf /etc/nginx/nginx.conf
